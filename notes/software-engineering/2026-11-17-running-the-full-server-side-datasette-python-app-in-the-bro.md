---
type: Article Summary
title: Running the full server-side Datasette Python app in the browser with service workers and Pyodide
description: Datasette Lite now runs the full server-side Datasette app in the browser by intercepting requests with service workers.
resource: https://simonwillison.net/2026/Nov/17/datasette-lite/
tags:
- software-engineering
- devex
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
sources:
- resource: https://simonwillison.net/2026/Nov/17/datasette-lite/
  title: Running the full server-side Datasette Python app in the browser with service workers and Pyodide
  author: Simon Willison
status: stable
source_url: https://simonwillison.net/2026/Nov/17/datasette-lite/
source: Simon Willison
source_type: blog
published: '2026-11-17'
domain: software-engineering
subdomain: devex
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d8107ab88c9fd556d93c7
---

## 🎯 Core thesis
Datasette Lite now runs the complete server-side Datasette Python application in the browser by using service workers to intercept requests, so it behaves almost exactly like a real Datasette server.

## 🧭 Why the author wrote it
The first Datasette Lite (2022) faked HTTP requests and pasted the results into the page, which broke any feature that relied on JavaScript. If the new design is faithful enough, a single link can reproduce a demo, a plugin, or a bug without anyone running a server, and the reader gets the same behavior a real deployment would show.

## Introduction: what the rewrite can do
Datasette Lite is the author's port of Datasette, the Python data-browsing application, to run in a browser using WebAssembly and Pyodide. The rewrite is the first version that can run almost any Datasette feature in the browser. Using carefully built URLs, you can:

- Load client-side Datasette to browse any SQLite database, JSON file, CSV file or SQL script
- Select a released version of Datasette to load from PyPI
- Load a specific commit, tag or branch name from the Datasette GitHub repository
- Pick from over 100 versions of SQLite to use with Datasette
- Install additional Datasette plugins from PyPI or from URLs to wheel files, including JavaScript visualization tools

The author notes that this makes it useful for linking to demos of work-in-progress features or bug reproductions, without spinning up a full web server.

### Datasette Lite
Datasette Lite is the browser-hosted build of Datasette, hosted at lite.datasette.io. It is a complete application, not a read-only viewer: the same routes, plugins and JavaScript features run as they would on a server. The author first launched it in 2022 as something of a joke, then found it useful in practice.

### Pyodide and WebAssembly
Pyodide is the project that makes Python run in the browser, and WebAssembly is the binary format browsers can execute at near-native speed. Together they let the Datasette Python code, and the SQLite engine underneath it, run on the visitor's machine with no backend.

> 🔎 Added context: Pyodide is CPython compiled to WebAssembly. WebAssembly is a compilation target that browsers run, so code written for other languages can be compiled and executed inside a page.

## Anatomy of a shareable URL
The author breaks down one example link to show what each part does. The full URL is:

```
https://lite.datasette.io/
  ?install=datasette-maplibre
  &url=https://.../demo.db
  &metadata=https://.../metadata.yml
  #/demo/capitals_and_countries%23map=0/4.2/180&color=continent
```

**What this does:**
- The host is Datasette Lite. The `?` query parameters are read before the application starts.
- `install=datasette-maplibre` installs a plugin from PyPI. You can repeat `&install=...` for as many plugins as you need.
- `url=` loads a SQLite database from a remote address. The file must send open CORS headers, which is why the author hosts these files in GitHub Gists.
- `metadata=` loads a metadata file that adds HTML descriptions to tables and to the homepage.
- The fragment after `#` is a location inside Datasette. `/demo/capitals_and_countries` opens a SQL view, and the second fragment, after the encoded `%23` (which is `#`), tells the map plugin to zoom to `map=0/4.2/180` and to color shapes by the `continent` column.

The example page shows the datasette-maplibre plugin running a database of GeoJSON country shapes, with a combined map and per-boundary maps inside a table. The author says the URL "is doing a lot of work", and he has written about the technique before.

### URL-addressable software
The author's idea is that the URL is the full description of a running environment: the Datasette version, plugins, data, metadata and view state are all encoded in the link. Anyone who opens the link gets the same setup, which removes the need for a setup guide. The author described this pattern in an earlier post on URL-addressable Pyodide Python environments. The full set of supported arguments is in the Datasette Lite README.

### CORS headers
CORS (Cross-Origin Resource Sharing) is the browser rule that blocks a page on one site from reading data fetched from another site unless the server allows it. The `url=` parameter only works when the data host sends the right headers, which is why the author uses GitHub Gists.

> 🔎 Added context: The browser enforces CORS, not the server. A file can be fetched directly with a tool and still be blocked when a web page reads it.

## Using it to reproduce bugs
The author already uses Datasette Lite during Datasette development. In a GitHub issue about a pagination bug, he posted two links. The first uses `ref=1.0a42` and shows the broken behavior: clicking "next" leaves the second page empty. The second uses `ref=1.0a43` and shows the same page with the fix applied. Both load the same SQL script from a Gist.

### Reproduction links
A reproduction link pins three things at once: the Datasette release (through `ref=`), the data (through `sql=` or `url=`), and the page location (through the fragment). Because the reader only has to click, a reviewer can compare the broken and fixed releases side by side without installing either. This is the practical value the author points to for work-in-progress features and bug reports.

## How it works: service workers
The author explains why the rewrite was needed by describing the first version, and then the current design.

### The original 2022 approach
The first version ran Datasette in a Pyodide web worker. It simulated HTTP requests through that stack, then inserted the HTML response into the top-level page with `innerHTML`. This worked for basic pages, but any feature that used JavaScript broke, because `<script>` tags inserted with `innerHTML` do not run. The web-worker workaround also could not load extra JavaScript or CSS files.

> ❌ **Anti-pattern — pasting responses into the page with innerHTML:** The app builds an HTML response in a worker and injects it into the live page. **Why people do it:** It is the simplest way to show output from a non-browser runtime. **Consequence:** Scripts and stylesheet references in the response do not behave as they would on a server, so interactive features fail silently. **Fix:** Intercept the browser's own requests with a service worker, so the page loads its assets through the normal request path.

### Service workers
Service workers are browser scripts that can intercept HTTP requests and answer them from client-side code. Datasette Lite registers one service worker for the whole lite.datasette.io domain. The parent page creates an iframe, and the iframe requests Datasette pages and assets from URLs of this form:

```
/app/aa94e6851595/                              # the homepage (HTML)
/app/aa94e6851595/data/places                   # a table page (HTML)
/app/aa94e6851595/-/static/navigation-search.js # (JavaScript)
/app/aa94e6851595/-/static/app.css              # (CSS)
```

**What this does:**
- These are standard Datasette paths, with an extra `/app/<session>/` prefix in front.
- The HTML, JavaScript and CSS requests all pass through the service worker, so Datasette's own static-file handling runs unchanged.
- The prefix is the key that lets one service worker serve many independent Datasette instances, explained below.
- Because the paths look like ordinary Datasette URLs, the application does not need special code for the browser.

> 🔎 Added context: A service worker stays registered in the browser and handles requests for its scope, and it can answer a request with a response it builds itself. That is what lets the page load its JavaScript and CSS from the client.

### Session prefix
The `/app/<session>/` prefix exists because of multiple tabs. The service worker is shared by every tab on the domain. If it routed every request to one Datasette instance, every tab would share that instance, whatever plugins or settings its URL asked for. The parent page instead generates a random session ID and uses it as the prefix for every hidden request. The service worker reads the session ID and passes the request back to the parent page, which forwards it to a dedicated Datasette instance for that session.

> ❌ **Anti-pattern — one shared runtime behind a shared router:** A single Datasette instance serves all requests on the domain. **Why people do it:** It is simpler and uses less memory. **Consequence:** Each tab's plugins, data and settings overwrite the others, so a demo link can show another demo's configuration. **Fix:** Put a per-session identifier in the URL path and route each request to its own runtime.

### Web workers are still needed
The author notes that Pyodide cannot run inside a service worker for long, because it cannot keep its state for more than around 30 seconds, depending on the browser. So each session's Datasette instance still runs in a web worker, and the service worker handles only the routing.

> 🔎 Added context: A web worker is a background JavaScript thread for long-running work, separate from the page. The browser can stop a service worker when it is idle, while a web worker lives for as long as its page does.

### Diagrams and history
The author asked Claude Fable 5.2 to generate several SVG diagrams to explain the design, and one is now in the Datasette Lite README. The system began in May 2026 as an AI research spike built with Claude Opus 4.8. The author said he did not feel confident enough to ship it until November 2026, when he used Claude Fable 5.2 to finish it.

## Bonus: choosing the SQLite version
The author added the ability to choose a SQLite version last night, as a side effect of investigating a possible SQLite bug. Datasette is designed to work with any released SQLite version since 3.25, which came out in September 2018. There have been 87 releases since then. Being able to run Datasette against a specific SQLite version helps demonstrate new SQLite features and debug odd issues. On a Mac, the author had previously needed various tricks to switch versions, which he describes as "not a trivial process".

### pysqlite3 WebAssembly wheels
The author built the wheels in a new repository, `datasette/pysqlite3-wasm-wheels`. It holds WebAssembly-compiled Python wheels for pysqlite3 covering 163 SQLite versions, with one file per version. The most recent version is SQLite 3.54.1.

```
pysqlite3-0.6.2-3.54.1-cp314-cp314-pyemscripten_2026_0_wasm32.whl
```

**What this does:**
- `pysqlite3-0.6.2` is the pysqlite3 Python package version.
- `3.54.1` is the SQLite version bundled inside the wheel.
- `cp314` means the wheel targets CPython 3.14.
- `pyemscripten_2026_0_wasm32` is the platform tag, meaning a WebAssembly build for the Pyodide environment.
- The filename encodes which SQLite version the wheel contains, so it can be looked up from a version number.

Datasette Lite accepts a special `?sqlite=` parameter. It takes a version number such as `3.54.1`, finds the matching wheel, fetches and installs it into the Pyodide environment, and configures Datasette to use it. The author also built a comparison tool that runs one SQL query across selected SQLite versions. His example, `select json_object('a', (select json_array(1)))`, produces different results on 3.54.0 and 3.54.1. The text does not explain the difference, which appears only in the screenshot.

## Beyond Datasette: the general pattern
The author argues that nothing about Datasette Lite is unique to Datasette. Almost any Python application could run this way, as a cheap way to provide preview environments, demonstrate features and recreate bugs. C and Rust extensions used to block this, but coding agents can now compile them to Pyodide-compatible WebAssembly. Those builds can be published to PyPI under PEP 783, which the author cites as available from April 2026.

### Native extensions in WebAssembly
Python packages with C or Rust code need to be compiled for the browser's WebAssembly target before Pyodide can load them. The author says an agent can now do this work, and PEP 783 provides a route to publish the results to PyPI. The author links the PyPI pull request that supports this.

### Databases as the limit
SQLite works without any extra effort, because it is an embedded library and the wheels above bundle it. Running PostgreSQL or MySQL inside WebAssembly is possible, but the author describes it as a path less trodden. The author hopes to see WebAssembly preview environments become a common pattern across the Python ecosystem.

## 🧠 Mental models
### Shared router, per-session runtime
**The model:** One service worker serves a whole domain, so a session ID in the URL path decides which isolated runtime handles each request.
**How to apply it:** When one shared front door must keep many users or tabs apart, put the session identity in the path and give each session its own stateful runtime.

### The URL is the full configuration
**The model:** A link can encode the software version, plugins, data source, metadata and view state, so the link alone reproduces an environment.
**How to apply it:** When sharing a demo or a bug report, put every variable that changes the behavior into the link rather than describing it in prose.

### Pin every version to reproduce
**The model:** Behavior depends on both the application release and the runtime version, such as the SQLite version, so a reproduction has to name both.
**How to apply it:** Run the same query against the versions that matter, for example 3.54.0 and 3.54.1, and discard the environment when the test is done.

## 🔑 Key takeaways
- Datasette Lite now uses service workers to intercept page and asset requests. This replaces the 2022 approach, which injected HTML with innerHTML and broke JavaScript features.
- Each browser session gets a random ID in its URL prefix, so tabs running different Datasette configurations stay separate even though one service worker serves the whole domain.
- Pyodide cannot keep its state inside a service worker for long, around 30 seconds depending on the browser, so each session's Datasette runs in a web worker.
- One URL can pin the Datasette release or commit, plugins, SQLite version, data, metadata and map view, which makes demos and bug reproductions shareable.
- The author demonstrated a pagination bug with two links, one on Datasette 1.0a42 showing the broken behavior and one on 1.0a43 showing the fix.
- The `?sqlite=` parameter works because 163 SQLite versions are packaged as WebAssembly wheels in the pysqlite3-wasm-wheels repository.
- The approach generalizes to most Python applications. The main gap the author names is server databases such as PostgreSQL and MySQL.

## 🏛️ For Kenji
> This is your interpretation, not the author's.

The pattern is a disposable preview environment: a link starts a faithful copy of a service with no infrastructure behind it. For a Staff-track architect, it could replace some per-branch preview deployments for services whose state fits in SQLite, and it gives reviewers a reproducible artifact for bug reports. The open question is how you would handle state in a server database such as PostgreSQL, which the article identifies as the hard part.

## 📚 Reading plan
### From the article
- [Datasette Lite](https://lite.datasette.io/) — the live tool, the fastest way to try the URL patterns.
- [URL-addressable Pyodide Python environments](https://simonwillison.net/2025/Feb/13/url-addressable-python/) — the author's earlier post on the URL technique.
- [Datasette Lite README](https://github.com/simonw/datasette-lite#load-a-different-datasette-version) — the full list of supported URL arguments.
- [Datasette Lite launch post (2022)](https://simonwillison.net/2022/May/4/datasette-lite/) — the original version and why it was built.
- [Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) — the browser mechanism the rewrite depends on.
- [CORS guide](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS) — why remote database files need open headers.
- [AI research spike](https://github.com/simonw/research/tree/main/pyodide-asgi-browser#readme) — the origin of the design, before it was productionized.
- [pysqlite3-wasm-wheels](https://github.com/datasette/pysqlite3-wasm-wheels) — the repository with the 163 SQLite wheels.
- [PyPI pull request for PEP 783](https://github.com/pypi/warehouse/pull/19804) — the change that lets Pyodide-compatible extensions be published to PyPI.

### To go further
- Pyodide project documentation — how CPython is built and run in WebAssembly.
- WebAssembly fundamentals — the compilation target behind Pyodide and the SQLite builds.
- Web Workers API — the background-thread model that hosts each session's Datasette instance.
- pysqlite3 — the Python bindings that the WebAssembly wheels are built from.
