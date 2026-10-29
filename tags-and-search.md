# Tags and Search

At the end of this chapter you can
file a link under several subjects at
once, and find it again three ways:
by a word, by a tag, and by the
unread flag.

**Check:** with the lab's five
bookmarks as the test collection,
searching `#reference` returns two,
`#python` returns one, and the word
`pangram` returns the pangram page —
the counts the lab recorded in
transcript t10.

## Tags instead of folders

A tag is a word you attach while
saving, in the form's **tag string**:
`reference language` is two tags. In
the database they are first-class
rows of their own — after the lab's
imports and saves, the tag list read
through the API was five names, each
with the date it first appeared:

```text
docs, imported, language, python, reference
```

(transcript t14.) Tags belong to the
collection, not to one bookmark: any
bookmark can reuse any tag, a tag
with no bookmarks left disappears
from the suggestions, and there is no
tag hierarchy to design. The filing
decision a folder tree forces — *the*
one place for this link — never
arises. A page about Python documentation
is under `docs` *and* `python`, and
both questions find it.

Three habits keep a tag set useful.
Keep tags lower-case and one word
(the tag string is split on spaces).
Prefer the plural question — "what
will I ask for?" — over describing the
page: `reference` earns its keep;
`interesting` does not. And let tags
stay few: five tags covered the lab's
collection; fifty would have been a
second job.

## Searching

One search box serves the list, in
the web page and through the API
(chapter 8), with one grammar:

- **Plain words** match against the
  title, description, notes and
  address. `q=pangram` found the
  pangram page; `q=python` found
  exactly the one bookmark whose title
  and tags mention it (transcript
  t10).
- **`#tag` asks for a tag exactly.**
  `#reference` returned the lab's two
  reference bookmarks, `#docs` one,
  `#language` one. Words and tags mix:
  `#reference tides` asks for the tag
  *and* the word.
- There is also a separate `tag`
  filter in the list's sidebar in the
  web page; the API expresses the same
  idea through the `#tag` search, and
  a bare `?tag=` parameter on the API
  is silently ignored — the lab's
  first attempt used it and got the
  whole list back, which is why this
  paragraph exists (transcript t10).

## The unread list is a reading queue

The **unread** flag of chapter 4
collects the links you saved in a
hurry. The list has an Unread view, and the
RSS feeds of chapter 7 include an
unread feed for reading the queue in a
feed reader. The working rhythm
this guide recommends: save generously
with Unread ticked, then clear the
queue deliberately — read the page,
finish the bookmark's description in
your own words, and untick.

Archiving (chapter 9) is a different
door from read: an **archived**
bookmark leaves the main list *and*
the search entirely. The lab proved
it by accident — a `#language` search
run while its only matching bookmark
was archived returned **0**, and the
same search after unarchiving
returned **1** (transcript t10).
Archived is not a softer delete; it
is a separate room, with its own list
and its own search.
