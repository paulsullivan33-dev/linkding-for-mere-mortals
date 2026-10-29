# Linkding for Mere Mortals

Every browser has bookmarks, and every
browser keeps them to itself: one list
per browser, per machine, sorted into
folders you made once and never
revisit. **Linkding** is the same idea
moved onto a small server you run
yourself. It is a web application —
one program, one database file — where
you save a link with a title, a
description, your own notes and a set
of **tags**. Then you can find it again
by searching words or tags, read it
later, share chosen links with other
people, and get the whole collection
back out as the same standard file
every browser can import. No account
with anyone, no subscription, and the
list is yours in a form you can carry
away.

This guide explains how Linkding holds
a bookmark, how to install it, and how
to do the everyday work: adding,
tagging, searching, importing a
browser's exported bookmarks, sharing,
and driving it from scripts through
its **REST API**. It also covers the
two kinds of archive Linkding knows —
putting a bookmark away in your own
archive, and asking the **Internet
Archive** to keep a snapshot of the
page itself.

The lab that wrote this guide ran the
real thing: **Linkding 1.47.0**, built
from its official source on a small
Linux machine, with its database,
background task worker and web pages
all exercised for real. Against it the
lab logged in through the real login
form, added bookmarks through the web
form and through the API, watched a
title stay empty and learned why,
imported a browser bookmarks file and
read the dates it carried, exported
everything back out, turned sharing on
and read the shared page *without
logging in*, created an API token the
way the settings page does it — after
first grabbing the wrong token and
being refused — and tested the edges:
a duplicate URL, a malformed one, a
deleted bookmark.

What the lab could not do, it did not
fake. Linkding's normal home is a
Docker container, and this sandbox
cannot run containers, so the Docker
install is taught from the project's
own documentation and labelled as
such; the from-source install the lab
actually ran is verified step by step.
Local page snapshots (which need a
headless browser on the server) are
likewise described, not run.
`criteria.md` keeps the full ledger of
what was run and what was only read.

## What this guide covers

- [**What Linkding is**](what-linkding-is.md) —
  the mental model: your links, on your
  server, in one database, found by
  search and tags instead of folders
- [**The moving parts**](the-moving-parts.md) —
  the web application, the SQLite
  database, the background task worker,
  the metadata reader, and which part
  holds what
- [**Installing Linkding**](installing-linkding.md) —
  the Docker route most people take,
  and the from-source route this lab
  verified, including the version and
  login checks that prove it runs
- [**Your first bookmark**](your-first-bookmark.md) —
  logging in, the add form field by
  field, and how Linkding offers to
  fetch a page's title for you
- [**Tags and search**](tags-and-search.md) —
  tags instead of folders, the `#tag`
  search, plain-word search, and the
  unread list as a reading queue
- [**Importing and exporting**](importing-and-exporting.md) —
  the Netscape bookmarks file every
  browser speaks: bringing a browser's
  bookmarks in, dates and tags intact,
  and taking the whole collection out
- [**Sharing and public bookmarks**](sharing-and-public-bookmarks.md) —
  the two switches that turn sharing
  on, the shared page, public sharing,
  and the RSS feeds — including the one
  whose address is itself a password
- [**The REST API**](the-rest-api.md) —
  API tokens (and the feed token that
  is *not* one), adding and editing
  from scripts, the duplicate that
  updates instead of duplicating, and
  the errors, quoted exactly
- [**Snapshots and the archive**](snapshots-and-archives.md) —
  two different archives: shelving a
  bookmark out of your list, and
  keeping a copy of the page itself
  through the Internet Archive
- [**Recipes**](recipes.md) — a reading
  queue that works, a household's
  shared shelf, a scripted nightly
  backup of the whole collection, and
  migrating in from a browser
- [**The neighbours**](the-neighbours.md) —
  the browser's own bookmarks, the
  read-it-later services, and a plain
  file of links: what each does that
  Linkding doesn't, and vice versa
- [**Limits**](limits.md) — no folders,
  no metadata for your internal pages,
  snapshots that live somewhere else,
  and a project that thinks in
  containers first
- [**Troubleshooting**](troubleshooting.md) —
  the database that cannot be opened,
  the token that is not a token, the
  title that never arrives, and the
  bookmark that vanished into the
  archive
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

Running Linkding for a large public
audience — it is a household-scale
tool, and chapter 12 is plain about
that. The browser extension's
development, single sign-on through an
identity provider (the machinery is
present in the dependencies and named
in the project's options; this lab ran
plain password logins), and building
the Docker image yourself. Hosting it
on the public internet safely — ports,
proxies and certificates — is pointed
at, not taught; the guides to reverse
proxies and Tailscale in this series
are the natural next step.

## How to use this guide

Read chapter 1 for the model, and
chapter 2 before installing anything —
knowing which part holds what turns
later confusion into a named problem.
Chapter 3 gets you a running server;
chapter 4 saves your first link in the
same sitting. Chapter 6 is the one to
read slowly: the import is how most
people's real collections arrive, and
the export is the promise that they
can leave. The lab's transcripts live
in `linkding-lab/transcripts/`.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [Calibre for Mere Mortals](https://paulsullivan33-dev.github.io/calibre-for-mere-mortals/)
- [Datasette for Mere Mortals](https://paulsullivan33-dev.github.io/datasette-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/linkding-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/linkding-for-mere-mortals/blob/main/LICENSE-CODE).
