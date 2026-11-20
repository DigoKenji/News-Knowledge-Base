---
type: Article Summary
title: PostgreSQL 19 Released
description: PostgreSQL 19 lets clients on a standby wait for a known WAL position, giving read-your-writes consistency without
  routing every read to the primary.
resource: https://simonwillison.net/2026/Nov/19/postgresql-19/
tags:
- data
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://simonwillison.net/2026/Nov/19/postgresql-19/
  title: PostgreSQL 19 Released
  author: Simon Willison
status: stable
source_url: https://simonwillison.net/2026/Nov/19/postgresql-19/
source: Simon Willison
source_type: blog
published: '2026-11-19'
domain: data
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d814a8f2dede89040e2df
---

## 🎯 Core thesis
PostgreSQL 19 lets an application read its own recent writes from a standby by having the standby wait until it has replayed the primary's write-ahead log up to a specific log sequence number (LSN), using the new `WAIT FOR LSN` command.

## 🧭 Why the author wrote it
Read traffic is often sent to replicas, but a replica can lag the primary, so a user may not see a change they just made. Simon Willison's point is that PostgreSQL now offers a direct answer to this problem, and that a long-standing pattern from Wikipedia's MediaWiki has a clean equivalent in PostgreSQL. If the thesis holds, applications can keep read scaling on standbys without giving up the "I see my own edit" guarantee.

## The release and its delay
The article is a link post. It reports that PostgreSQL 19 was released on 19 November 2026 (linked to the official release announcement, also discussed on Lobsters). Most years PostgreSQL ships in September. This year's release arrived two months later, which the author attributes mainly to the volume of issues the PostgreSQL team had to address during what he calls the "ongoing AI-reported vulnerability apocalypse". A further linked blog post on rhaas.blogspot.com covers the delay in more detail. The author says the release "was definitely worth the wait" and names this feature as his favorite.

> 🔎 Added context: A standby is a replica that continuously applies changes from the primary. Release delay itself is not a technical topic in this article; the rest of the note focuses on the feature.

## Read-your-writes on standbys with WAIT FOR LSN
The author's central point is that PostgreSQL 19 adds "read-your-writes" consistency for applications that send reads to standbys. The new `WAIT FOR LSN` command is the mechanism. A session on a standby can run `WAIT FOR LSN` to pause until the standby has replayed the write-ahead log (WAL) up to a given log sequence number (LSN). Only after that point does the session run its query. The result is that the application can read data it has just written on the primary, even though the read happens on a replica.

> 🔎 Added context: Read-your-writes is a session guarantee: a client always sees the effects of its own earlier writes. WAL is the ordered log of every change; an LSN is a position in that log, so waiting for an LSN means waiting for a precise point in the change history, not for a rough time.

## Using WAIT FOR LSN in an application
The article shows the pattern as two steps. First, after a write on the primary, the application captures the current WAL insert position. The example writes an update and then asks for the position:

```sql
UPDATE movie SET genre = 'Dramatic' WHERE genre = 'Drama';
SELECT pg_current_wal_insert_lsn();
```

The author notes this returns a value like `0/0306EE20`, and suggests storing it somewhere the next request can reach, such as a signed cookie. Second, on a subsequent request the application waits on the standby before it reads:

```sql
WAIT FOR LSN '0/0306EE20' WITH (TIMEOUT '0.1s');
SELECT * FROM movie WHERE genre = 'Drama';
```

**What this does:**
- The first statement changes data on the primary and captures the WAL position that identifies that change, so the position is known before the read.
- The LSN is passed with the next request rather than kept on the server, which is why the author suggests a signed cookie. The token travels with the client.
- `WAIT FOR LSN` blocks the standby session until replay reaches the stored position. The query then runs against a state that includes the write.
- `WITH (TIMEOUT '0.1s')` bounds the wait. The timeout is optional. Without one, the wait is open-ended. With one, the author says you get an error if the replica does not catch up in time, so the application must decide what to do then.

> 🔎 Added context: The article does not say what an application should do after the timeout error. A common choice is to fall back to the primary, but that is a design decision for the application, not something the article specifies.

## Prior art: the MediaWiki replication wait
The author first met this pattern in Wikipedia's software. MediaWiki has a hand-rolled version of it against MySQL replication. Its guarantee is about chronological order: a user should see wiki events in order across requests. A few seconds of lag is acceptable, as long as subsequent requests show a consistent picture. The mechanism saves the primary's binlog position in the user's session. At the start of each request, the application waits for the replica to reach that saved position before doing any reads from it.

The author links the MediaWiki code, which now uses MySQL's `WAIT_FOR_EXECUTED_GTID_SET()` and falls back to `SOURCE_POS_WAIT()` for older setups. The structure is the same as the PostgreSQL command: remember a position on write, wait for a replica to reach it before reading. PostgreSQL 19 brings that structure into the database itself, under a command.

> 🔎 Added context: MySQL's binlog position and GTID sets, and PostgreSQL's LSN, all name a point in the replication stream. The shape of the solution is the same even though the details differ.

## 🧠 Mental models
### Capture a position on write, wait for it on read
**The model:** A write produces a position in the replication log. A client that carries that position with its next request can ask a replica to wait until it has reached that point before reading.
**How to apply it:** After a write on the primary, read the current WAL position and store it with the client's session or in a signed cookie. Before reading from a standby on the next request, wait for that position, with a bounded timeout.

### Waiting should be bounded and the fallback decided
**The model:** Waiting for a replica is useful only while it is short. A timeout turns an unbounded wait into a known failure mode the application can handle.
**How to apply it:** Choose a timeout that matches your user-facing latency budget. Decide in advance whether a timed-out read goes to the primary, returns a stale result, or shows an error.

## 🔑 Key takeaways
- PostgreSQL 19 provides read-your-writes consistency for reads sent to standbys through the `WAIT FOR LSN` command.
- The application captures the write position with `pg_current_wal_insert_lsn()` after the write and reuses it on the next request.
- `WAIT FOR LSN` blocks a standby session until replay reaches the given LSN, so the following query sees the write.
- The timeout clause is optional. With a timeout, a replica that does not catch up in time produces an error the application must handle.
- The pattern is not new: MediaWiki already has a hand-rolled version of it, built on a saved MySQL binlog position and a replica wait.
- The PostgreSQL 19 release was two months later than usual, mainly because of the security issue load the author describes.

## 🏛️ For Kenji
This is your interpretation, not the author's. In a read-scaling architecture with standbys, the consistency gap between primary and replica is a design concern that usually gets handled ad hoc in each service. If your teams route reads to replicas, `WAIT FOR LSN` moves part of that logic into the database and gives you a standard mechanism to govern. The question it raises: where should the LSN token live (client cookie, session store, or request header), and what should a timed-out read do, given that the fallback choice changes both latency and consistency?

## 📚 Reading plan
### From the article
- [PostgreSQL 19 Released](https://www.postgresql.org/about/news/postgresql-19-released-3449/) - the official release announcement, the primary source for the release itself.
- [via](https://lobste.rs/s/pqyuhk/postgresql_19_released) - the Lobsters discussion the author found the release through.
- [thanks mainly](https://rhaas.blogspot.com/2026/10/postgres-19-is-late-and-its-ais-fault.html) - the linked blog post on why the release was late.
- [`WAIT FOR LSN`](https://www.postgresql.org/docs/19/sql-wait.html) - the command reference for the feature at the center of the article.
- [log sequence number (LSN)](https://www.postgresql.org/docs/19/wal-internals.html) - the WAL internals documentation needed to understand LSN positions.
- [in Wikipedia](https://www.mediawiki.org/wiki/Manual:Database_access#Lag) - MediaWiki's documentation of database access and replication lag, the prior art.
- [the relevant MediaWiki code](https://github.com/wikimedia/mediawiki/blob/1ff78194b182ee4b5b3678f7d9fc9aefd0852cf7/includes/libs/Rdbms/Replication/MysqlReplicationReporter.php#L238-L299) - the actual replication wait implementation for MySQL.
- [WAIT_FOR_EXECUTED_GTID_SET()](https://dev.mysql.com/doc/refman/9.7/en/gtid-functions.html#function_wait-for-executed-gtid-set) - the MySQL function MediaWiki now uses for the wait.
- [SOURCE_POS_WAIT()](https://dev.mysql.com/doc/refman/8.4/en/replication-functions-synchronization.html#function_source-pos-wait) - the older MySQL fallback for the same wait.

### To go further
- Read-your-writes and other session guarantees - the consistency model this feature targets, and how it differs from stronger guarantees.
- Replication lag handling in read-scaling architectures - how other databases and services decide between replica reads and primary reads.
