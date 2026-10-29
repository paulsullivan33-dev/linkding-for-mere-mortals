# Installing Linkding

At the end of this chapter a Linkding
server answers on your machine, an
admin account exists, and you have
seen its login page with your own
eyes.

**Check:** request the login page and
get the title `Login - Linkding` back
with an `HTTP 200`, then log in with
the account you created. The lab's
version of both checks is in
transcripts t03 and t05.

## Two routes, honestly labelled

Linkding's home route is **Docker**:
the project ships a container image,
and its documentation's install
section is that one command plus a
folder for the data. That route is
*documented-only* in this guide — the
lab's sandbox cannot run containers,
so no transcript claims it. What the
lab verified end to end is the second
route: running **from source**, the
same code the container carries. The
project also publishes a bare-metal
install script for Debian and Ubuntu
(`install-linkding.sh` in the source);
the lab read it but did not run it.

## What the source route needs

Two things surprise people. First,
**Python 3.13 or newer**: version
1.47.0 declares `requires-python =
">=3.13"`, and one of its safety
features (chapter 2's address guard)
relies on 3.13's address handling. A
machine with only Python 3.12 — the
lab's starting point — must fetch a
newer Python first; the lab used the
**uv** tool, which downloads an
interpreter of its own (transcript
t01). Second, the **frontend build**:
the styled pages need one `npm`
command (chapter 2). Nothing else is
exotic — the database is SQLite, so
there is no database server to
install.

## The verified path

Fetch the source (the lab took the
official repository at version
**1.47.0**, commit `27b7303`):

```bash
git clone --depth 1 https://github.com/sissbruecker/linkding.git
cd linkding
```

Make an environment and install the
dependencies the project declares.
The lab used pip in a virtual
environment and deliberately left out
two declared packages — **uwsgi** (a
production app server) and
**supervisor** (a process manager) —
because the development server and
task runner used here do not need
them; a production source install
should include both:

```bash
python3.13 -m venv ~/linkding-venv
~/linkding-venv/bin/pip install \
  beautifulsoup4 bleach bleach-allowlist \
  django djangorestframework huey markdown \
  mozilla-django-oidc requests waybackpy
```

(transcript t02; the project's own
route, `uv sync`, installs the full
locked set, uwsgi and supervisor
included.) Build the frontend:

```bash
npm ci
npm run build
```

(transcript t04.) Create the data
folder the fresh clone lacks, then let
Django build the database — this is
the step whose absence causes the
`unable to open database file` error
of chapter 13:

```bash
mkdir -p data data/previews data/favicons data/assets
python manage.py migrate
```

The migrations end with `OK` lines,
and `data/` now holds `db.sqlite3`
and `tasks.sqlite3` (transcript t03).

## The first account, and starting up

The **superuser** — the first admin —
is created non-interactively by naming
the password in an environment
variable for this one command:

```bash
DJANGO_SUPERUSER_PASSWORD='choose-a-real-one' \
  python manage.py createsuperuser \
  --noinput --username admin --email you@example.org
```

`Superuser created successfully.` is
the receipt (transcript t03). The
lab's password was a throwaway
(`LabPass!234`) for an instance that
no longer exists; yours should be a
real one, kept wherever you keep
passwords.

Start the two processes — the web
server and the task worker of
chapter 2:

```bash
python manage.py runserver 127.0.0.1:8931
python manage.py run_huey
```

Binding to `127.0.0.1` keeps this
first run reachable only from the
machine itself, which is the right
posture until you have decided how
the server will be reached (a reverse
proxy, a VPN such as Tailscale, or
your home network only). The check:

```bash
curl -s http://127.0.0.1:8931/login/ | grep -o '<title>[^<]*</title>'
# <title>Login - Linkding</title>
```

Chapter 4 takes it from the login
form.
