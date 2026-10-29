# The Neighbours

Linkding keeps links, and it is not
the only thing that does. These are
characterisations from the projects'
own documentation and the lab's
verified edges — the lab ran Linkding
only, and checked the series index
before pointing at any sibling guide.

## The browser's own bookmarks

The incumbent. Already installed,
already syncing if you let the browser
maker do it, saved with a keystroke —
and organised as a folder tree, one
list per browser, with descriptions
and notes nowhere and search that
barely deserves the name. Linkding's
whole argument is chapters 1 and 6:
tags instead of folders, one list on
your server, an exit file. But the
browser wins the moment of saving, and
Linkding knows it — which is why it
ships a **bookmarklet** (a small
bookmark that opens the add form with
the current page filled in; it sits
on the Integrations settings page) and
a **browser extension**, so that
saving to Linkding is as close to
`Ctrl+D` as a separate server gets.
The lab drove the web form the
bookmarklet opens, not the bookmarklet
itself — that part is documented-only
here.

## The read-it-later services

Pocket made the pattern famous, and
its successors — Wallabag (self-hosted,
like Linkding), Instapaper, Raindrop —
centre on *reading*: they fetch the
article's text, restyle it, and keep a
copy for offline reading. If your list
is mostly articles you intend to read
once, that is the better tool, and
Wallabag is the one that keeps the
self-hosting property. Linkding
centres on *finding again*: it keeps
the reference, the tags and your
notes, and its copy of the page —
when you ask for one — lives at the
Internet Archive or in a snapshot file
(chapter 9), not in a reading view.
The unread queue of chapter 5 borrows
the read-it-later shape without the
machinery.

## Other self-hosted bookmark managers

**Shaarli** is the elder: one PHP
file-era tool, links with tags and a
daily permalink page, no database
server needed, smaller and simpler
than Linkding and without its API
story. **Linkace** is the Laravel
counterpart, heavier, with link
checking and monitoring built in. The
choice among the three is mostly
about which machinery you already
run; this guide exists because
Linkding's combination — one SQLite
file, a real REST API, the Netscape
exit — is the one the lab could
verify smallest.

## A plain file of links

The zero-software neighbour: a text
file, or the Netscape export itself,
kept in a synced folder. It has no
search to speak of, no sharing, no
API — and it will outlive every tool
in this chapter, which is why
chapter 6 treats the export as a
first-class feature rather than a
migration detail. Several sibling
guides in this series keep the same
faith with plain formats: the Calibre
guide's library is files on disk, and
the Datasette guide publishes a
SQLite file as a website — which, if
your bookmark database ever needs a
public face beyond chapter 7, is a
surprisingly good way to give it one.
