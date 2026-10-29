# Troubleshooting

The failures the lab actually met,
plus the ones its evidence predicts,
each with the check that names it.

## `unable to open database file`

**Symptom:** the first `migrate` (or
any command) dies with
`sqlite3.OperationalError: unable to
open database file`.
**Cause:** a fresh source clone has no
`data/` folder, and SQLite will not
invent the directory for the file it
is asked to open.
**Fix:** `mkdir -p data` (plus the
`previews`, `favicons` and `assets`
subfolders) in the installation, then
rerun. Transcript t03 has both the
failure and the fix.

## The API says `Invalid token.`

**Symptom:** every API request returns
`{"detail": "Invalid token."}` even
though you are sending *a* token.
**Cause, most likely:** you sent the
**feed token** — the string inside the
RSS addresses on the Integrations
page. It is not an API token.
**Check:** compare with the token the
**Create API token** form showed once
when you created it. If it is lost,
create a fresh named token and revoke
the old one; tokens are independent.
A `401` instead means no token arrived
at all — check the header. Transcript
t09 keeps the lab's own walk into this
one.

## A bookmark has no title, and never gets one

**Symptom:** a saved bookmark sits in
the list as its bare address.
**Causes, in order:** (1) You saved it
without a title through a script or
the API — the title-fetch happens in
the browser's form flow (chapter 4),
not on save; edit one in, as
transcript t10 does by `PATCH`.
(2) The address is internal — your own
network — and the guard of chapter 2
refused the fetch (`"title": null`
from the check endpoint, transcript
t08). Write the title yourself; that
is the guard working, not a bug.

## Settings will not save (sharing stays off)

**Symptom:** you tick the sharing
boxes, save, and nothing changes.
**Cause the lab found:** the settings
page carries several forms posting to
one address, and the server chooses
what to update by the *submit control
included in the post* — scripted
submissions that omit it are accepted
and ignored. In a browser this cannot
happen; with `curl` or a script, post
the real form, button and all.
**Check:** read the profile flags
back, or log out and try
`/bookmarks/shared` after enabling —
transcript t12 shows the flags at 0
after the incomplete post and 1 after
the complete one.

## A search finds nothing that is there

**Symptom:** `#tag` returns 0 for a
bookmark you can see in the full list.
**Likely cause:** the bookmark is
**archived**. Archived bookmarks leave
the main search entirely (transcript
t10). Look in the archive view, or
unarchive it.
**Second suspect:** the API parameter.
`?tag=` is ignored; tags are searched
inside `q` as `#tag` (chapter 5).

## `404` on the shared page

**Symptom:** `/shared` returns "Page
not found".
**Cause:** the address is
`/bookmarks/shared`. If that also
404s or shows nothing, work chapter
7's switches in order: sharing on,
public sharing on (for anonymous
visitors), and the bookmark's own
Shared flag set.

## Snapshots never appear

**Symptom:** the Internet Archive
integration is on, but no snapshot
addresses fill in.
**Checks:** is the **task worker**
running (`python manage.py run_huey`,
or the worker half of your service)?
Snapshot requests are queue work —
with no worker, they wait forever
(chapter 2). Then check the page is
public: the worker's fetching obeys
the same address guard as chapter 4,
and archive.org cannot save what it
cannot reach either.

## Pages arrive unstyled

**Symptom:** a from-source install
works, but every page is bare HTML.
**Cause:** the frontend bundle was
never built (chapter 2).
**Fix:** `npm ci && npm run build` in
the source tree, then reload.
Transcript t04 lists the files that
should exist under `bookmarks/static/`.
