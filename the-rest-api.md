# The REST API

At the end of this chapter a script
can do everything the web pages do:
create a named API token, add and edit
bookmarks, search them, archive and
delete them — and read the errors well
enough to know what went wrong.

**Check:** the lab's closing API
sequence (transcript t14) lists five
tags, re-adds an existing address and
gets the *same* bookmark back updated,
deletes a bookmark with `204`, and
then gets `404` asking for it. Your
script reaching the same four answers
has the whole model.

## Tokens first — and the trap

Programmatic access uses an **API
token**: a named credential you create
in **Settings → Integrations**, under
**REST API**, with the **Create API
token** form. Give it a name that says
which script it belongs to
(`lab-script`, in the lab). The key is
shown **once**, when created; store it
where the script can read it and
treat it like a password, because it
is one — the settings page itself
warns that any party with the token
can manage all your bookmarks. Tokens
are listed by name on the same page
and can be revoked there, one by one,
without disturbing the others.

The trap sits centimetres away on the
same page: the **feed token** inside
the RSS addresses of chapter 7. It is
also a long hexadecimal string, and it
is *not* an API token. The lab scraped
the page, grabbed the wrong string,
and the API answered every request
with:

```json
{"detail": "Invalid token."}
```

No token at all answers `401`
instead (transcript t09). Two refusals,
two different causes — invalid versus
absent — and telling them apart is the
first API skill.

Requests carry the token in a header:

```bash
curl -s -H "Authorization: Token <your-token>" \
  http://127.0.0.1:8931/api/bookmarks/
```

## The working set

Everything below was run against the
lab (transcripts t10, t14):

- **Create** — `POST /api/bookmarks/`
  with JSON: `url`, `title`,
  `description`, `notes`, `tag_names`
  (a list), `unread`, `shared`. The
  answer is the stored bookmark with
  its new `id` and `date_added`.
- **List and search** — `GET
  /api/bookmarks/` returns a count and
  a page of results; `?q=` takes the
  search grammar of chapter 5, `#tag`
  included.
- **Edit** — `PATCH /api/bookmarks/<id>/`
  changes named fields only. The lab
  used it to give its title-less
  bookmark a title, and to share a
  bookmark in chapter 7.
- **Archive and unarchive** — `POST
  /api/bookmarks/<id>/archive/` and
  `.../unarchive/`, answering `204`;
  archived bookmarks list separately at
  `/api/bookmarks/archived/`.
- **Delete** — `DELETE
  /api/bookmarks/<id>/`, answering
  `204`; asking for the bookmark
  afterwards answers `404`.
- **Tags** — `GET /api/tags/` lists
  the tag set with first-appearance
  dates.

## The duplicate that updates

The API's most useful surprise: adding
an address that already exists does
not create a copy and does not fail.
The lab posted `https://www.python.org`
a second time with a new title and got
back `HTTP 201` — and the *same*
bookmark, `id` 3, its title updated
and its description refreshed from the
page itself:

```json
{"id": 3, "url": "https://www.python.org",
 "title": "Python.org again",
 "description": "The official home of the Python Programming Language",
 ...}
```

(transcript t14.) For scripts this is
exactly right: a nightly job can
re-submit links it cares about without
first asking which exist. For humans
it explains the occasional bookmark
that seems to have edited itself — a
script re-added it.

Validation is equally plain. A
malformed address answers with the
field named and the reason:

```json
{"url": ["Enter a valid URL."]}
```

No stack trace, no mystery — the API
is a well-behaved citizen, and a
script should check the status code
before parsing the body.
