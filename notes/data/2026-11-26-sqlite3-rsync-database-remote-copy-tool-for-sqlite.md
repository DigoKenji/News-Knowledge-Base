---
type: Article Summary
title: 'sqlite3_rsync: Database Remote-Copy Tool For SQLite'
description: sqlite3_rsync copies or syncs SQLite databases over SSH, sending only differing pages and avoiding full local
  duplicates.
resource: https://simonwillison.net/2026/Nov/26/sqlite3-rsync
tags:
- data
- database-dbms
- replication-partitioning
- delta-sync
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-26T06:47:00-03:00'
sources:
- resource: https://simonwillison.net/2026/Nov/26/sqlite3-rsync
  title: 'sqlite3_rsync: Database Remote-Copy Tool For SQLite'
  author: Simon Willison
status: stable
source_url: https://simonwillison.net/2026/Nov/26/sqlite3-rsync
source: Simon Willison
source_type: blog
published: '2026-11-26'
domain: data
concepts:
- database-dbms
- replication-partitioning
- delta-sync
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/424baf74172d8106bb78d3472d9b70c7
---

## 🎯 Core thesis
sqlite3_rsync can copy a snapshot of a SQLite database to another machine over SSH without first making a full local duplicate, and it can resynchronise an existing copy by sending only the content that differs.

## 🧭 Why the author wrote it
Simon Willison knew sqlite3_rsync as an experimental SQLite tool, but he says he only understood one of its most attractive properties on this day. The problem it starts from is that the usual ways to snapshot a database need free disk space equal to a whole second copy, which is painful on small instances. If the thesis is right, the disk constraint no longer forces you to choose between a snapshot and a full local copy.

## The tool and its history
The article describes sqlite3_rsync as an additional SQLite tool. It was added as "experimental" in [SQLite 3.47.0 in October 2024](https://www.sqlite.org/releaselog/3_47_0.html) and enhanced in [SQLite 3.50.0 in May 2025](https://www.sqlite.org/releaselog/3_50_0.html). The author's own discovery was not the tool's release but one of its characteristics, which he understood only recently.

### sqlite3_rsync
The tool is named after rsync, the Unix utility that synchronises files by transferring only what changed. The article's claim is that the same idea applies to a SQLite database file, with two uses: taking a snapshot of a live database remotely, and bringing an existing remote copy up to date cheaply. The tool is documented in full at [the sqlite3_rsync page on sqlite.org](https://www.sqlite.org/rsync.html).
> 🔎 Added context: rsync is a long-standing Unix file-synchronisation tool. It compares the source and destination and sends only the differences, instead of copying everything again.

## The snapshot problem with existing methods
Normally, the article says, a snapshot copy of a SQLite database is made with one of two methods. Both require enough disk space on the machine doing the copy to hold a complete duplicate of the database. On a small instance that already uses more than half of its disk, the author says, you are "plain out of luck".

### Backup API
The article links to [the SQLite backup API](https://www.sqlite.org/backup.html) as the first way to snapshot a database. The author treats it as a normal, supported route and its limitation is space, not correctness.
> 🔎 Added context: the backup API is SQLite's interface for copying one database into another, and it works on a live database. The article does not describe its internals, so only the disk-space limitation stated in the article should be relied on.

### VACUUM INTO
The second method is [VACUUM INTO](https://www.sqlite.org/lang_vacuum.html#vacuuminto), which writes a copy of the database to a new file. Like the backup API, it needs room for that new file, so it has the same disk-space cost.
> 🔎 Added context: VACUUM INTO is an SQL statement. You run it against an open database and name a file path, and SQLite writes the copy there.

> ❌ **Anti-pattern — staging a full local copy before moving it:** Creating a complete duplicate of the database on the same machine, then transferring that file elsewhere. **Why people do it:** the two built-in snapshot routes (the backup API and VACUUM INTO) both write a full file, so it looks like the only option. **Consequence:** on a small instance with little free disk, the copy cannot be made at all, as the article warns. **Fix:** stream the snapshot straight to the destination over SSH with sqlite3_rsync, so no full local duplicate is needed.
> 🔎 Added context: the article does not label this as an anti-pattern. It is inferred from the disk-space constraint the author describes.

## Snapshot over SSH without a local copy
The author's key claim is that sqlite3_rsync "can copy a snapshot of your database over an SSH connection to another machine, without first needing to create a full local copy". The value is clearest on constrained hosts, where the source machine cannot afford a second copy of the database to exist even briefly.

### Remote snapshot over SSH
A remote snapshot here means a consistent copy of the database taken at one point in time and delivered to another machine. SSH (Secure Shell) is the encrypted channel between the two machines that carries it.
> 🔎 Added context: SSH is an encrypted protocol for running commands and moving data between computers over a network. The article only says the connection is SSH and does not describe the security properties beyond that.

## Incremental synchronisation of an existing copy
The second use is keeping a copy that already exists on the other machine up to date. The article says this "works in a similar way to regular rsync", and it gives the core idea of the protocol in a block quote:

> The core idea behind the protocol is that the replica sends cryptographic hashes for pages or groups of pages over to the origin side. The origin sends back page content that differs, or requests finer-grain hashes if a multi-page hash does not match. If the origin and replica start out being vastly different, the total network bandwidth can exceed the size of the entire database, due to the overhead of exchanging hashes, but the excess bandwidth is not great - a few percent at most.

### Hash-based delta sync
Delta sync means transferring only the parts of the data that differ between two copies. Here the replica (the destination copy) computes hashes over pages, which are the fixed-size storage units of the database file. A hash is a short fingerprint of a block's content, and any change to the block changes its fingerprint. The replica sends these fingerprints to the origin, which compares them with its own and replies with the page content that differs. When a hash covering a group of pages does not match, the protocol asks for finer hashes on that group, narrowing the search until only the changed pages are sent.
> 🔎 Added context: "cryptographic hash" here means a fingerprint that is practically impossible to forge or collide. Its strength matters less for the bandwidth argument than the fact that it is compact and changes whenever content changes.

The article's numbers are the important part. When the two copies are nearly identical, traffic is only the hashes plus the few changed pages. When they start out vastly different, the hash exchange itself adds overhead, and the total can exceed the database size. The article argues that this excess is small, a few percent at most, so the protocol is never a poor bargain in the worst case it describes.

> ❌ **Anti-pattern — re-sending the whole database to refresh a replica:** Copying the entire database file over the network every time the remote copy needs updating. **Why people do it:** it is the simplest approach and needs no protocol knowledge. **Consequence:** network traffic and transfer time scale with the full database size, even when only a few pages changed. **Fix:** let the replica send page hashes and transfer only the pages that differ, as sqlite3_rsync does.
> 🔎 Added context: the article does not label this as an anti-pattern. It is inferred from the author's statement that sqlite3_rsync avoids re-transmitting the entire database.

## Update: the author's earlier coverage
The author adds that he had already written about this synchronisation use in October 2024, when the tool was first released. The [earlier post](https://simonwillison.net/2024/Oct/4/sqlite-rsync/) is the reference for that earlier discussion. The article does not say what the post contained beyond the fact that it covered this application of sqlite_rsync, so no further detail is given here.

## 🧠 Mental models
### Compare fingerprints, then drill down
**The model:** Two copies first exchange compact hashes of coarse groups of data. Only the groups whose hashes disagree are examined further, down to individual pages, so the work and traffic follow the differences rather than the total size.
**How to apply:** When you need to know what differs between two large stores, compare summaries first and descend only into the parts that disagree.

### Stream instead of stage
**The model:** Move a snapshot across the network as it is produced, instead of building a complete intermediate copy on the source and then moving that copy.
**How to apply:** When disk headroom is tight, check whether a transfer tool can read the source and write to the destination directly before assuming you need a staging copy.

## 🔑 Key takeaways
- sqlite3_rsync can take a snapshot of a database over SSH without first creating a full local copy, where the backup API and VACUUM INTO need room for a complete duplicate.
- Synchronising an existing copy avoids re-transmitting the whole database, because the origin sends back only the page content that differs.
- The protocol exchanges hashes of pages or groups of pages, and it asks for finer-grained hashes when a group's hash does not match.
- When two copies start out vastly different, total network bandwidth can exceed the size of the entire database, because of the hash exchange overhead.
- The article puts the excess bandwidth beyond the database size at a few percent at most.
- The tool was added as experimental in SQLite 3.47.0 (October 2024) and enhanced in SQLite 3.50.0 (May 2025).
- The author had already written about this synchronisation use in October 2024, when the tool was first released.

## 🏛️ For Kenji
> 🔎 This is your interpretation, not the author's. In architecture work, this is relevant to backup, disaster recovery and refreshing replicas of SQLite files in small services or edge nodes, where a second full copy may not fit on the disk. One question it raises: how much does delta sync cost for a database that is written continuously, where changes touch many pages and the copies often drift far apart?

## 📚 Reading plan
### From the article
- [sqlite3_rsync: Database Remote-Copy Tool For SQLite](https://www.sqlite.org/rsync.html) — the official documentation for the tool itself.
- [SQLite 3.47.0 release notes](https://www.sqlite.org/releaselog/3_47_0.html) — where sqlite3_rsync was added as experimental.
- [SQLite 3.50.0 release notes](https://www.sqlite.org/releaselog/3_50_0.html) — the May 2025 enhancements.
- [SQLite backup API](https://www.sqlite.org/backup.html) — the first snapshot method the article compares against.
- [VACUUM INTO](https://www.sqlite.org/lang_vacuum.html#vacuuminto) — the second snapshot method, which writes a copy to a new file.
- [Simon Willison's October 2024 post on sqlite-rsync](https://simonwillison.net/2024/Oct/4/sqlite-rsync/) — the author's earlier coverage of the synchronisation use.

### To go further
- rsync's delta-transfer algorithm — the file-level idea that sqlite3_rsync adapts to database pages.
- Hash trees (Merkle trees) — the general structure behind comparing data through hierarchical hashes.
- Snapshot strategies for embedded databases — when to stage a copy and when to stream one.

## Concepts
- [database-dbms](/concepts/data/database-dbms.md)
- [replication-partitioning](/concepts/backend-systems/replication-partitioning.md)
- [delta-sync](/concepts/data/delta-sync.md)
