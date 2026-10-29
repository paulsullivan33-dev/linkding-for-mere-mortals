# Your First Bookmark

At the end of this chapter your first
link is saved with your own title,
description and tags, it sits in the
list marked unread, and you know why a
bookmark saved *without* a title may
keep the empty space where the title
should be.

**Check:** open the bookmark list and
see your link's title; then read the
collection back through the API the
way transcript t10 does and find the
same entry with its tags attached.

## Logging in

The login is an ordinary web form:
name, password, and a **CSRF token** —
a hidden field that proves the
submission came from the form the
server served. Browsers handle that
invisibly. The lab drove it with
`curl` to show there is no other
machinery: fetch the form, keep the
cookies, post the three fields, and
the answer is a redirect to the list:

```text
POST /login/ -> HTTP 302, redirect: http://127.0.0.1:8931/bookmarks
GET /bookmarks -> HTTP 200
<title>Bookmarks - Linkding</title>
```

(transcript t05.) A wrong password is
refused on the same form; there is no
separate "API login" to learn.

## The add form, field by field

**Add bookmark** presents these fields
(the lab listed them from the served
form, transcript t05):

- **URL** — the address, the only
  required field. It is validated: the
  API's version of a bad one answers
  `Enter a valid URL.` (chapter 8).
- **Title** and **Description** —
  yours to write, or fetched from the
  page (next section).
- **Notes** — a longer private text;
  not shown in the list's compact view.
- **Tag string** — tags typed as
  words: `reference language` files
  the bookmark under both.
- **Unread** — a checkbox that marks
  the bookmark as to-read.
- **Auto close** — closes the form's
  tab after saving, for the
  bookmarklet flow of chapter 11's
  neighbours.

The lab's first save, with tags
`reference language` and Unread
ticked, came back as a redirect to a
list that showed its title
(transcript t06). One field is
conspicuously absent: **Shared**.
Sharing is off for the whole
installation until a switch in
Settings turns it on, and only then
does the form offer it (chapter 7).

## Where titles come from

Save a bookmark with the title left
empty and Linkding stores it empty —
the lab's second bookmark sat in the
list as its bare address,
`tides.html`, and stayed that way
(transcripts t06, t07). The title you
expected arrives by a different road:
when you paste an address into the
form in a browser, the page quietly
asks the server to peek at the page —
the **check endpoint**:

```bash
curl -s -b cookies.txt \
  "http://127.0.0.1:8931/api/bookmarks/check/?url=https://example.com"
```

```json
{"bookmark": null,
 "metadata": {"url": "https://example.com",
              "title": "Example Domain",
              "description": null,
              "preview_image": null},
 "auto_tags": []}
```

(transcript t08.) The browser fills
the form from that answer; a script
that skips the check, as the lab's
did, gets no such favour. Two honest
consequences:

- A bookmark saved without a title
  keeps no title until you edit one
  in. Chapter 8 does exactly that
  through the API.
- The peek obeys chapter 2's guard.
  The same check against the lab's own
  page server returned
  `"title": null` — the guard will not
  fetch loopback or private-network
  addresses. Links to your router, NAS
  or home services save perfectly
  well; you just write their titles
  yourself.

## The shape of success

After two saves the database itself
told the story (transcript t07):

```text
(1, 'http://127.0.0.1:8932/pangrams.html', 'Pangrams - Sentences That Use Every Letter', 1)
(2, 'http://127.0.0.1:8932/tides.html', '', 0)
tags: [('language',), ('reference',)]
```

Row 1 has its title and its unread
flag (`1`); row 2 has neither. Chapter
6 is about finding these again —
by tag, by word, and by that unread
flag.
