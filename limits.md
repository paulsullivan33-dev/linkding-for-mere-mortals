# Limits

What Linkding cannot do, stated in one
place so the rest of the guide's
enthusiasm can be trusted.

**No folders.** Tags are the only
organisation. If your mind files by
hierarchy, Linkding will feel flat;
the tag habits in chapter 5 are the
mitigation, not a cure.

**No metadata for your own network.**
The address guard of chapter 2 refuses
to fetch titles, descriptions and
favicons from loopback, private and
link-local addresses. Bookmarks to
your router, NAS and home services
save and search normally, but you
write their titles yourself — and the
guard's relaxation
(`LD_ALLOWED_INTERNAL_HOSTS`) is a
deliberate weakening you should apply
per-host, not a switch to flip.

**Snapshots live somewhere else, or
cost heavily.** The Internet Archive
integration keeps copies on
archive.org, under its rules; whether
any single save succeeds is outside
Linkding's control, and the lab could
verify the submission path, not
authorship of the resulting capture.
Local snapshots keep copies on your
disk but require a headless Chromium
and megabytes per page — machinery
this guide documents rather than runs.

**One collection per user, shared by
flags.** There are no shared
collections with joint ownership, no
comments, no per-tag permissions.
Households share by the mechanism of
chapter 7 — each person shares their
own bookmarks onto a common page —
which is enough for a shelf and not a
collaboration platform.

**The phone experience is the web
page and an extension.** There is no
Linkding app of its own to read
offline; the unread queue is a web
view. Read-it-later tools (chapter 11
of the neighbours) exist because this
is a real gap.

**The project thinks in containers.**
Docker is the supported install, the
manual's centre of gravity, and the
route that gets the tested
combination of versions. The source
route works — this guide is the
evidence — but you become your own
integrator: Python 3.13's arrival, the
frontend build, and the two processes
(web and worker) are yours to keep
running. Relatedly, versions move:
the lab verified **1.47.0**; options
and page layouts in a later release
may differ in detail.

**Household scale.** One small Django
application with a SQLite file is a
deliberate design, and it is sized for
a person or a family. Hundreds of
bookmarks are nothing to it; a public
bookmarking service for thousands of
strangers is a different tool's job.
