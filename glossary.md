# Glossary

**API token** — a named credential,
created in Settings → Integrations,
that authenticates scripts against
the REST API. Shown once at creation;
revocable by name. Not the feed token.

**Archive (a bookmark)** — to shelve
a bookmark out of the main list and
search without deleting it. Reversed
by unarchiving. A different idea from
a snapshot.

**Background task worker** — the
second Linkding process (`run_huey`)
that works through queued jobs:
snapshot requests, favicon refreshes,
cleanup. If it is not running, queued
jobs wait.

**Bookmarklet** — a small bookmark in
your browser that opens Linkding's
add form with the current page already
filled in.

**Check endpoint** —
`/api/bookmarks/check/`, the address
the add form asks for a page's title
and description when you paste a URL
in a browser.

**CSRF token** — a hidden form field
proving a submission came from a form
the server served. Ordinary web
hygiene; it appears in chapter 4
because the lab logged in with curl.

**Django** — the Python web framework
Linkding is built with. It provides
the pages, the login system, the
database layer and the admin.

**Favicon** — the small icon a website
shows in a browser tab. Linkding
fetches and stores them for bookmarks
where the address guard allows.

**Feed token** — the credential inside
your RSS feed addresses. It reads
feeds and does nothing else; the API
refuses it. Treat the addresses as
passwords.

**Huey** — the task-queue library
Linkding's background worker is built
on. Its queue is `data/tasks.sqlite3`.

**Internet Archive / Wayback Machine**
— archive.org's library of saved web
pages. Linkding's snapshot integration
asks it to save copies of bookmarked
pages.

**Netscape bookmarks file** — the
HTML-based export format every browser
speaks (`NETSCAPE-Bookmark-file-1`),
carrying each link's address, save
date (`ADD_DATE`) and, where present,
tags and a description.

**Public sharing** — the profile
switch that lets visitors with no
account see the shared page and the
public feed. Off by default, separate
from sharing itself.

**REST API** — Linkding's programmatic
door: JSON over HTTP at `/api/`, for
bookmarks, tags and user details.

**Shared** — a per-bookmark flag
marking it for the shared page. Only
exists once sharing is enabled in the
owner's settings.

**single-file** — an external program
that saves a web page as one
self-contained HTML file, used by
Linkding's local snapshots together
with a headless Chromium.

**Snapshot** — a copy of a bookmarked
*page*, kept by the Internet Archive
or locally as a file. A bookmark is
the reference; a snapshot is the
evidence.

**SQLite** — the database that is a
file. Linkding's whole collection is
`data/db.sqlite3`.

**SSRF** — server-side request
forgery: tricking a server into
fetching addresses on an internal
network. Linkding's guard against it
is why internal pages get no fetched
titles.

**Tag string** — the space-separated
tags typed into the add form:
`reference language` files a bookmark
under both.

**Unread** — a per-bookmark flag for
"saved to read later"; the unread
view and feed make it a reading queue.
