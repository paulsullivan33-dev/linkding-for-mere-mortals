# Importing and Exporting

At the end of this chapter your
browser's existing bookmarks live in
Linkding — with the dates you saved
them and the tags the file carried —
and you have taken the whole
collection back out as a file any
browser can import.

**Check:** after the lab's import,
the API listed **5** bookmarks where
there had been 3, and the two new ones
carried 2025 dates from the file
itself (transcript t11). After the
export, the downloaded file contained
all 5 addresses.

## The Netscape file

Every browser can export its bookmarks
to one ancient, stable format: an HTML
file that begins

```html
<!DOCTYPE NETSCAPE-Bookmark-file-1>
```

and lists each bookmark as a link with
attributes:

```html
<DT><A HREF="https://example.org/" ADD_DATE="1735689600" TAGS="reference,imported">Example Domain (example.org)</A>
<DD>A placeholder domain used in documentation examples.
```

Three parts of that line matter to
Linkding. **HREF** is the address.
**ADD_DATE** is when the link was
saved, as a Unix timestamp — Linkding
*keeps* it, so an imported collection
arrives with its history instead of
five hundred bookmarks all dated
today. **TAGS**, a comma list, becomes
real tags; browsers that file by
folder instead write the folder path,
which the importer maps as it can —
the settings form also offers a
**map private flag** option for files
whose `PRIVATE` attribute should be
honoured.

## Importing, as the lab did it

In **Settings → Import**, choose the
file and submit. The lab's file held
two entries — a documentation example
domain and the RFC Editor — and the
result, read through the API, was:

```text
4 | Example Domain (example.org) | imported,reference | added: 2025-01-01
5 | RFC Editor                   | docs,imported      | added: 2025-01-02
```

(transcript t11.) The dates are the
file's own `ADD_DATE` values
(1 January and 2 January 2025), the
tags arrived as tags, and the `<DD>`
description text came with them. An
address that is already in the
collection is not duplicated — the
importer recognises it the way the
API does (chapter 8).

Importing is also the honest answer to
"how do I start?": not by re-saving
years of links one at a time, but by
exporting from the browser you have
and bringing the file over. Do it
once, then tidy tags gradually —
chapter 5's advice about few tags
applies double to an imported
collection, which tends to arrive
with folder names as tags.

## Exporting, and the exit promise

**Settings → Export** downloads the
same format: the lab's export was
1,252 bytes containing all five
bookmarks as `<DT>` entries with their
addresses (transcript t11). That file
imports into Firefox, Chrome, Safari,
and back into a fresh Linkding.

Treat the export as more than a
migration tool. It is the collection
in a form no database can hold
hostage: run it monthly (chapter 10
scripts it through a full backup
instead) and Linkding can fail, be
reinstalled, or be abandoned without
your links failing with it. A bookmark
manager whose entire value is *your
list* should make the list easy to
take away, and this one does — the
lab checked, rather than trusting the
settings page's word for it.
