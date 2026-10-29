# Recipes

Four jobs, each assembled from parts
the earlier chapters verified. Where a
step was not itself run in the lab,
the recipe says so.

**Check:** every recipe ends with the
observation that proves it worked —
repeat that observation, not just the
steps.

## A reading queue that empties

1. Save links generously, **Unread**
   ticked and one tag for the subject
   (`reference`, say). In a browser,
   the bookmarklet and extension of
   chapter 11's neighbours make this a
   two-click habit.
2. Once a week, open the **Unread**
   view and work it: read the page,
   rewrite the description in your own
   words — the sentence you will
   actually search for later — and
   untick Unread.
3. For links whose moment passed
   unread, **archive** rather than
   delete (chapter 9); they leave the
   queue without leaving the
   collection.

Done means: the unread count is a
number you could clear in one sitting,
and nothing in it is older than your
patience.

## A household shelf

1. Create one account per person
   (admin page; the lab ran a single
   admin, so multi-account behaviour
   beyond the shared page is
   documented-only).
2. Each person turns on **Enable
   bookmark sharing** in Settings;
   the household agrees that **public
   sharing stays off** unless the
   server is meant to be public.
3. Share the links everyone needs —
   school and council pages, the good
   manual, the shared hobby references
   — by ticking **Shared** on each.
4. Everyone reads the shelf at
   `/bookmarks/shared`, logged in.

Done means: a second person, logged
in as themselves, sees the shared
titles — and none of your unshared
ones.

## Scripted nightly backup

Two layers, both worth having:

```bash
# the database itself (stop the server first, or copy both files together)
cp data/db.sqlite3 data/tasks.sqlite3 /backup/linkding-$(date +%F)/

# the portable form: the export of chapter 6, via the API token of chapter 8
curl -s -H "Authorization: Token <your-token>" \
  "http://127.0.0.1:8931/api/bookmarks/?limit=1000" \
  > /backup/linkding-bookmarks-$(date +%F).json
```

The first preserves everything —
users, tokens, settings. The second is
the collection in a form any script
can search and any future tool can
import after conversion; the Settings
export of chapter 6 is its browser
equivalent. The lab verified the
pieces (database size and contents,
API listing, the export file) rather
than this exact script, which is
assembled from them.

Done means: restore once, somewhere
scratch, and count the bookmarks —
a backup you have not restored is a
hope, not a backup.

## Migrating in from a browser, gently

1. Export the browser's bookmarks to
   its Netscape file (chapter 6).
2. Import it into Linkding untouched.
   Accept the mess: folder names and
   stray tags included.
3. Over the following weeks, fix tags
   during normal use — when a search
   fails to find a link you know is
   there, open it, give it the tags
   your failed search just taught you,
   and save.
4. Keep the browser export file. It is
   the before-picture, and the format
   means the road back exists
   (chapter 6 again).

Done means: your ten most-reached-for
links are findable by the words you
actually tried — the only metric an
import should be graded on.
