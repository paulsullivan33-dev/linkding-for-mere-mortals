# Sharing and Public Bookmarks

At the end of this chapter, bookmarks
you choose are visible to the rest of
your household on a shared page — and,
if you turn the second switch on, to
anyone who can reach the server —
while everything you did not choose
stays private.

**Check:** with sharing on, the lab
opened `/bookmarks/shared` *without
logging in* and read the shared
bookmark's title (transcript t12). If
your anonymous request is refused or
the page is empty, a switch below is
off.

## Two switches, deliberately separate

Sharing does not exist in a fresh
Linkding. In **Settings** (the profile
form), two checkboxes create it:

- **Enable bookmark sharing** — the
  shared page and the per-bookmark
  **Shared** flag appear. Other
  *logged-in users* can now see
  bookmarks whose owners marked them
  shared.
- **Enable public sharing** — the
  shared page and the public feed open
  to visitors with no account at all.

The lab's path to those checkboxes
carries a lesson worth the paragraph:
the settings page holds several forms,
and its update is chosen by the
*submit button's name* in the posted
data (chapter 13 tells the story).
Submitted properly, the profile flags
read back as on (transcript t12).

Even with both switches on, a bookmark
is shared only when its own **Shared**
flag is set — sharing is per-bookmark
and opt-in every time. The lab shared
exactly one of its five (Python.org,
through the API: `PATCH` with
`{"shared": true}`, chapter 8).

## The shared page

Shared bookmarks live at
`/bookmarks/shared`. Logged in, the
lab found its one shared title there;
**logged out**, the same:

```text
GET /bookmarks/shared (anonymous) -> HTTP 200
<title>Shared bookmarks - Linkding</title>
```

with the title present (transcript
t12). Note the address: `/shared`
alone is a 404 — the page hangs off
the bookmarks path, and the lab's
first guess is recorded so yours need
not be. What an anonymous visitor
sees is titles, descriptions and tags
of shared bookmarks: a reading shelf,
not your collection. Nothing unshared
leaks into it — the other four lab
bookmarks never appeared.

## The feeds

**Settings → Integrations** also lists
RSS feeds of your bookmarks, in four
flavours: **all**, **unread**,
**shared**, and the **public shared**
feed. Their addresses teach the
security model better than any
paragraph:

- The first three contain your **feed
  token** in the path —
  `/feeds/<token>/all` and its
  siblings. The token *is* the
  credential: anyone holding the
  address can read the feed, which is
  what a feed reader needs, and the
  settings page says so in plain words
  and tells you the token can be
  deleted (in the admin panel) to mint
  a fresh one if an address escapes.
- The public feed, `/feeds/shared`,
  has no token because it needs none:
  it carries only shared bookmarks
  from users who enabled public
  sharing. The lab fetched it
  anonymously before public sharing
  was on and got a valid but *empty*
  feed; after, the same address
  carried the shared bookmark
  (transcript t12).

One warning the lab earned and
chapter 8 repeats: the feed token and
the **API token** are different
strings with different powers. The
feed token reads feeds. It does not
authenticate the API, and presenting
it there earns `Invalid token.`
