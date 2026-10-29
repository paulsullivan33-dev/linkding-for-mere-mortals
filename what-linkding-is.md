# What Linkding Is

You have a link — an article, a manual
page, a recipe, a tool you will need
again in six months — and you want it
*kept*: findable, described in your own
words, and not buried in whichever
browser you happened to be using.
**Linkding is a bookmark manager that
runs on a server you control.** You
open its web page, paste the link, add
a title, a description and some tags,
and it stores the lot in a database on
that server. Later — from any browser,
on any machine that can reach the
server — you search a word or click a
tag and the link is in front of you.

**Check:** after reading this chapter
you can say what a Linkding bookmark
contains (an address, a title, a
description, notes, tags, and a few
flags), and name the two ways a link
gets in — by hand in the web page, and
from a script through the API — which
chapters 4 and 9 teach.

## The shape of the idea

A browser's bookmarks are a *tree*:
folders inside folders, one home per
link, and a filing decision every time
you save. Linkding replaces the tree
with a *list plus tags*. A bookmark
can carry several tags at once —
`reference` and `language`, say — so
the same link turns up under both
questions without being copied or
filed twice. Finding is searching:
type words that appear in the title,
description or address, or search
`#reference` to ask for a tag exactly.
Chapter 5 works this through with the
lab's own five bookmarks.

The second difference is where the
list lives. A browser's bookmarks live
in that browser, and the sync services
that move them between machines belong
to the browser's maker. Linkding's
list lives in one file — a **SQLite
database** — on your server. Backing
it up is copying that file (chapter 3
measures the lab's whole database at
266,240 bytes). Moving away is one
export in Settings, which writes the
standard **Netscape bookmarks file**
that every browser can import
(chapter 6). Nothing about the format
locks you in.

## What a bookmark holds

In the lab's finished collection, one
bookmark looked like this, read back
through the API:

```json
{"id": 3, "url": "https://www.python.org",
 "title": "Python.org",
 "description": "The Python programming language home page.",
 "notes": "", "unread": true, "shared": false,
 "tag_names": ["docs", "python"],
 "date_added": "2026-10-29T18:50:30Z"}
```

The **address** and **title** are the
essentials. The **description** is a
line or two in plain words — yours, or
the page's own description when
Linkding fetches it (chapter 4).
**Notes** are a longer private text.
**Tags** are the filing system.
**Unread** is a flag you set when a
link is a promise to your future self,
and chapter 5 uses the unread list as
a reading queue. **Shared** decides
whether this bookmark may appear on
the shared page (chapter 7) — off by
default, and sharing itself is off
until you switch it on.

## Who it is for

The person whose real reference
library has quietly become four
browsers' worth of unsorted bookmarks.
The household that wants one shelf of
"the good links" everyone can read.
The script-writer who wants to file a
link from the command line and search
the collection from a program — the
API in chapter 8 is a first-class
door, not an afterthought. The common
thread is one person, or one household,
with links they intend to find again.

## The honest shape, up front

Linkding is deliberately small. There
are no folders, no ratings, no
comments from visitors, no app for
your phone beyond its web page and a
browser extension for saving. It does
not keep a copy of every page you save
by itself: the snapshot feature asks
the Internet Archive to keep one, or
uses a heavy extra program to make one
locally (chapter 9). And it will not
fetch titles for pages on your own
home network — a deliberate safety
rule, explained where the lab met it
in chapter 4. Chapter 12 collects the
limits in one place.
