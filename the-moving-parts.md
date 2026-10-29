# The Moving Parts

Linkding looks like one program, but
five parts do the work, and knowing
which part holds what turns later
confusion into a named problem. This
chapter names them, in the order a
bookmark meets them.

**Check:** you can point at the part
responsible for each of these — the
web page not loading, a title that
never fills in, an export file, an API
request being refused — before you
install anything.

## The web application

Linkding is written in **Django**, the
Python web framework. It serves every
page you see — the login form, the
bookmark list, the settings — and the
REST API of chapter 8 from the same
program. In the lab it ran under
Django's own development server:

```bash
python manage.py runserver 127.0.0.1:8931
```

and answered the login page with
`HTTP 200` and the title
`Login - Linkding` (transcript t05).
Production installs put a real web
server in front of the same
application; the pages and the rules
do not change.

The pages themselves are styled by a
small **frontend bundle** — one
JavaScript file and two theme
stylesheets, built from the source
with a tool called esbuild. The lab
built them:

```text
bookmarks/static/bundle.js        151,828 bytes
bookmarks/static/theme-light.css   56,303 bytes
```

(transcript t04). A from-source
install that skips this step still
works — every form and list functions
— but the pages arrive unstyled,
which is why chapter 3 includes the
build.

## The database

Everything you save lives in one
**SQLite** file, `data/db.sqlite3`
inside the installation. Bookmarks,
tags, users, settings, API tokens —
one file, no database server to run.
The lab's finished database, holding
five bookmarks, five tags, one user
and two tokens, was 266,240 bytes
(transcript t14). A fresh clone of the
source has no `data/` folder at all,
and the first run fails with
`sqlite3.OperationalError: unable to
open database file` until you create
it — the lab met exactly that
(transcript t03), and chapter 13 keeps
the fix.

## The background task worker

Some jobs are too slow to do while a
page waits: submitting snapshot
requests, refreshing the little
website icons (**favicons**), tidying
up. Linkding queues those in a second,
smaller SQLite file (`data/tasks.sqlite3`)
and a separate worker process — the
**task runner**, built on a library
called Huey — works through them:

```bash
python manage.py run_huey
```

The lab ran it and watched it start
(transcript t07) and later saw the
snapshot tasks in its list by name
(transcript t13). The practical lesson
is in chapter 9: if the worker is not
running, queued jobs simply wait.
Nothing is lost; nothing happens,
either.

## The metadata reader

When you save a public web page,
Linkding offers to fetch its title and
description for you. A part called the
**website loader** does that fetching,
and it is deliberately fussy about
where it will go. All requests to
addresses *you* supplied go through a
guard whose own source states its
rule: it refuses to connect to
addresses that are not publicly
routable — **loopback, private
networks, link-local** — as protection
against a class of attack called
**SSRF**, where a server is tricked
into fetching things on an internal
network (transcript t08 reads the
guard's own header comment). The lab
proved both halves: a public page
(`https://example.com`) came back with
its title, `Example Domain`; the lab's
own test page on the same machine came
back with no title at all, because the
guard would not fetch it. An option
(`LD_ALLOWED_INTERNAL_HOSTS`) can
relax the guard for named hosts; the
default is the safe one, and this
guide leaves it alone.

## Tokens, feeds and snapshots

Three smaller parts recur through the
later chapters:

- **API tokens** are named credentials
  you create in Settings, one per
  script or program, and revoke the
  same way (chapter 8). A different
  string on the same settings page —
  the **feed token** — belongs to the
  RSS feeds and is *not* an API token;
  the lab confused them once, and the
  API's refusal (`Invalid token.`) is
  quoted in chapter 8 so you can
  recognise it.
- **RSS feeds** publish your bookmarks
  — all, unread, or shared — to feed
  readers, at addresses that contain
  the feed token (chapter 7).
- **Snapshots** are copies of the
  *page*, as opposed to the bookmark.
  Linkding can ask the **Internet
  Archive** to save one, or — with an
  extra program and a headless
  browser — make one itself
  (chapter 9).
