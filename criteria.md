# Writing criteria for this guide

Every "for Mere Mortals" guide is written to the same nine-point
standard, so a reader who trusts one can trust the rest. Here it
is, with the verification notes for this particular guide below.

1. **Fix it first.** Reader outcomes lead, not action items: each
   chapter opens by naming the outcome it delivers, shows a check
   the reader can run to confirm they got it, and a reasonable
   reader finishes able to state what was done, what it means,
   and what to try next.
2. **Plain language.** Short sentences, everyday vocabulary,
   technical terms defined on first use. No slang, and no
   idioms that only make sense in one country — the reader may
   be translating as they go.
3. **No dead ends.** A guide never tells the reader to do
   something it cannot show them how to verify. Every claim a
   reader relies on — an install, a command, an output — is
   something this guide demonstrates or clearly labels
   documented-only below.
4. **Live-verified where it matters.** Procedures are run for
   real in the guide's lab wherever the build machine allows it,
   and the transcripts kept in the repository are the evidence.
   What could not be run is listed honestly below, never
   presented as if it had been.
5. **General audiences.** Nothing in this guide — examples,
   fixtures, names, addresses — is modelled on any particular
   reader or author's real life, network, or organisations.
   The lab's sentences are stock sentences and pangrams.
6. **Standalone.** A reader needs this guide, its lab, and
   nothing else. Where another guide would genuinely help, it
   is pointed to once, by name, and the chapter still completes
   without it.
7. **Honest about limits.** Chapter 12 names what the tool
   cannot do: no folders, no fetched metadata for internal
   addresses, snapshots that live on someone else's service or
   need a headless browser the lab did not run, sharing by
   per-bookmark flags only, and a project whose supported
   install is a container this guide documents rather than runs.
8. **The ethics rule is in the text, early.** Linkding's own
   lines are drawn plainly and before procedures depend on
   them: chapter 2 explains the address guard that refuses to
   fetch internal URLs, chapter 7 states that public sharing is
   a separate switch that is off by default, and chapters 7–8
   repeat that feed addresses and API tokens are credentials to
   be kept like passwords.
9. **Builds clean.** The site builds with MkDocs in strict mode
   with no warnings, and a fresh reader following the chapters
   in order reaches the stated outcome of each.

## Verification notes for this guide

The build machine is a small Ubuntu 24.04 sandbox (shared with
other work; the sandbox cannot run containers, and its stock
Python is 3.12). **Linkding 1.47.0** was cloned from the
project's official repository (commit
`27b7303baf41bb28babc610ac8eaa486e1ddfab5`, 2026-09-13) and run
**from source, unmodified**: a CPython 3.13.18 interpreter
already present on the machine, a pip virtual environment
installing the project's declared dependencies (Django resolved
to 6.1.2) **minus uwsgi and supervisor**, which the development
server and task runner used here do not need, and the frontend
bundle built with the project's own `npm run build`. The
server ran on loopback (`127.0.0.1:8931`) against two lab pages
served on a second loopback port and several public reference
pages. All transcripts live in `linkding-lab/transcripts/`
(t00–t14). Lab credentials (`LabPass!234`, the API and feed
tokens printed in transcripts) belonged to the disposable lab
instance only, which was deleted after the run.

Run for real, with transcripts as evidence:

- Clone, version and declared requirements read from the
  source itself, including `requires-python = ">=3.13"` (t00,
  t01) and the dependency install (t02).
- Database creation: the fresh clone's missing `data/` folder
  produced the genuine `unable to open database file` failure;
  after creating it, all migrations applied and the superuser
  was created; `data/db.sqlite3` and `data/tasks.sqlite3`
  exist (t03).
- Frontend build producing `bundle.js` (151,828 bytes) and the
  two theme stylesheets (t04).
- The full web flow with curl and a cookie jar: login form,
  CSRF, `POST /login/` → 302 to the list, the add form's field
  names read from the served page (t05); two bookmarks added
  through the form — one titled and unread, one untitled, which
  stayed untitled in the list and the database (t06, t07).
- The background worker (`run_huey`) started and observed; it
  does not fill in web-form titles (t07).
- The check endpoint: a public URL returned its real title
  (`Example Domain`); the loopback lab page returned
  `"title": null`, traced to the shipped SSRF guard in
  `bookmarks/services/http_client.py`, whose header comment is
  quoted in chapter 2 (t08).
- API tokens: the RSS **feed token** was mistaken for an API
  token and refused with `Invalid token.`; no token answers
  401. A real token was then created through the settings
  page's own create-token form, shown once (t09, t10).
- The REST API end to end: create, list, search (`q=python`
  → 1), tag search inside `q` (`#reference` → 2, `#docs` → 1),
  the ignored bare `?tag=` parameter, PATCH editing, archive
  (204) hiding a bookmark from search (count 0) and unarchive
  restoring it (count 1), archived list, delete (204) then
  GET → 404 (t10, t14).
- Import of a two-entry Netscape file: both entries landed,
  `TAGS` became tags, the `<DD>` text became descriptions, and
  `ADD_DATE` was honoured (added dates read back as
  2025-01-01 and 2025-01-02). Export returned all five
  bookmarks as a 1,252-byte Netscape file (t11).
- Sharing: enabling it required the settings form's own
  submit field (an incomplete scripted post was accepted and
  silently ignored — flags stayed 0, then read 1 once posted
  properly); one bookmark shared; `/bookmarks/shared` read
  **anonymously** with HTTP 200 and the shared title present;
  `/shared` alone is a genuine 404; the public RSS feed was
  empty before public sharing and carried the shared bookmark
  after; the token'd private feed answered 200 (t12).
- Internet Archive integration: switched on in the profile,
  a bookmark added, the worker run with the snapshot tasks
  registered, and the Wayback availability API answering that
  a snapshot of the page exists (status 200, available). The
  guide claims the Linkding-side path only; it does not claim
  the lab's submission authored that capture (t13).
- API edges: re-adding an existing URL returned 201 with the
  **same** bookmark id, title updated and description
  refreshed from the page; a malformed URL answered
  `{"url": ["Enter a valid URL."]}`; the tag list and the
  final database size (266,240 bytes) recorded (t14).

Documented only — described from the project's documentation
or shipped source, not run, and the text does not claim
otherwise:

- The **Docker install**, the project's supported route, and
  its bare-metal install script: the sandbox cannot run
  containers, and the script was read, not executed.
- **Local HTML/PDF snapshots** (`LD_ENABLE_SNAPSHOTS` with the
  single-file program and a headless Chromium): the settings
  and task code were read in the source (t13); no local
  snapshot was produced.
- The **browser extension and bookmarklet** in a real browser:
  the form they open was driven directly; the browser side was
  not.
- **Multiple user accounts** beyond the single lab admin, and
  single sign-on (OIDC): the dependency ships with the project;
  logins here were password logins for one user.
- Favicon and preview-image fetching for public sites, email
  or other notifications, and running under uwsgi/supervisor
  or a reverse proxy for real users.
- The neighbour tools of chapter 11 (Shaarli, Linkace,
  Wallabag and the read-it-later services) — characterised
  from their documentation, not run.

Fixtures are this guide's own and deliberately generic: two
tiny lab-written reference pages (pangrams, tide tables) served
locally, public documentation sites (python.org, example.org,
the RFC Editor) as bookmark targets, and a two-entry import
file written for the lab. No real person's bookmarks, history
or organisation appears anywhere in the lab.
