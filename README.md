# Gate — Event Registration (Flask)

A small, self-contained event registration app: browse events, register with
a simulated payment step, and get a ticket. Anyone with access to the running
app sees the same event list and live spot counts; each visitor's own
tickets are tracked separately (via a session cookie), so "My Tickets" only
shows what *they* registered for.

## Run it

```bash
pip install -r requirements.txt
python app.py
```

Then open **http://localhost:5000** in a browser. To let other people on
your network join, replace `localhost` with your machine's local IP address
(the server already binds to `0.0.0.0`).

## Admin password

Adding new events requires a password, currently `gate2026`. Change it
before sharing the app with anyone by setting an environment variable:

```bash
export ADMIN_PASSWORD="your-own-password"
python app.py
```

## Data storage

Events and tickets are stored in `data.json` in this folder — it's created
automatically on first run, seeded with a few sample events. Delete it any
time to reset the app back to the sample data.

This is a simple flat-file store meant for local or small-scale use (a
single lock protects concurrent writes). It is **not** built for high
concurrency or production traffic — if you outgrow it, swap `_load_data` /
`_save_data` in `app.py` for a real database (SQLite is a natural next
step).

## Notes

- Payment is fully simulated — card fields are validated for shape only,
  nothing is ever charged.
- `SECRET_KEY` (used to sign session cookies) also defaults to a dev value —
  set your own via the `SECRET_KEY` environment variable before deploying
  anywhere other people can reach.
- There's no real user accounts/auth here — "My Tickets" is tied to an
  anonymous session cookie, not a login.
