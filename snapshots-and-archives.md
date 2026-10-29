# Snapshots and the Archive

Two different ideas in Linkding share
the word *archive*, and confusing them
causes the one real fright this tool
offers — a bookmark "vanishing". This
chapter separates them. At its end you
can shelve a bookmark out of your list
and bring it back, and you know what
Linkding can and cannot keep of the
*page itself*.

**Check:** archive one bookmark and
watch the main list's count drop by
one while the archived list's rises by
one (transcript t10). The bookmark was
never gone; it moved rooms.

## The first archive: your own shelf

Every bookmark can be **archived** —
removed from the main list, the unread
queue and the search — without being
deleted. The web page offers it per
bookmark; the API does it with

```bash
curl -s -X POST -H "Authorization: Token <your-token>" \
  http://127.0.0.1:8931/api/bookmarks/1/archive/
# -> HTTP 204
```

and reverses it with `unarchive/`.
The lab archived its pangram bookmark
and then re-ran a tag search that had
previously matched it: **0 results**,
then **1** again after unarchiving
(transcript t10). Think of the archive
as the box in the attic: for links
whose moment has passed but whose
deletion you are not sure of. Delete
(chapter 8) is the other door, and it
is final.

## The second archive: a copy of the page

A bookmark is an address, and
addresses rot. **Snapshots** are
Linkding's answer: a copy of the page
as it was, kept somewhere. There are
two mechanisms, and they are not
interchangeable.

**The Internet Archive integration**
is the lighter one. Turn on **Web
Archive integration** in Settings, and
when a bookmark is added the
background worker (chapter 2) submits
the address to the Internet Archive's
**Wayback Machine**, asking it to save
a copy. The bookmark gains a
`web_archive_snapshot_url` — a Wayback
address for that page — which the API
showed the lab even before the
integration was switched on, as the
place a snapshot *would* live:

```text
https://web.archive.org/web/20261029185030/https://www.python.org
```

The lab enabled the integration,
added a bookmark, ran the worker, and
then asked the Wayback availability
API about the page: a snapshot exists
and is available (transcript t13).
What this does *not* prove — and the
guide will not claim — is that the
lab's own submission created that
particular copy; popular pages are
archived by many hands. What is
verified is the whole Linkding side of
the path: the switch, the queue, the
task, and the recorded snapshot
address. The copy itself lives on
archive.org, under its rules and
retention, not on your server.

**Local snapshots** are the heavier
mechanism: with the option
`LD_ENABLE_SNAPSHOTS` turned on,
Linkding can save a page as a single
self-contained HTML file (and PDF or
screenshot forms) into `data/assets/`,
using an external program called
**single-file** driving a headless
Chromium browser. The settings file
in the source names both the option
and the program (transcript t13 reads
them). This route is **documented-only**
in this guide: the lab had no Chromium
and did not install one, so no local
snapshot was made or is claimed. It is
the route to choose when the copy must
live on your own disk; be aware it
makes each snapshot megabytes, not
bytes, and the browser it drives is
the largest thing in the whole
installation.

## Which to use

For most households: the in-app
archive for tidiness, and the Internet
Archive integration for pages whose
loss would hurt — documentation,
references, the article everything
cites. Local snapshots are for pages
you cannot trust anyone else to keep,
priced in disk and machinery. A
bookmark with neither is still a
bookmark; chapter 12 is honest that
Linkding's core promise is the *list*,
kept well — the copies are guests.
