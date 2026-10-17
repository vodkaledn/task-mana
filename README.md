# TaskDeck

A single-file task manager for people juggling many projects. No build step, no server, no account — open `index.html` in a browser and it works.

## Features

- **Compact task form** — task + project, date + time, urgency + repeat, paired two to a row.
- **Projects with colours** — assign each task to a project (or create one inline) and give the project a colour from an 8-swatch palette. The colour shows as a stripe and dot on every task, in the week calendar, and on the project's progress bar; click a project's dot to reveal the palette and recolour it.
- **Drag to reorder** — inside a project, drag tasks by the ≡ grip into whatever order you want to work them; the order is saved.
- **Urgency symbols** — mark a task `!` low, `!!` medium or `!!!` high. The symbol appears on the task everywhere, colour-coded grey / amber / red, and more urgent tasks sort first within the same time slot.
- **Any date** — a built-in Monday→Sunday month picker (no locale-dependent native popup).
- **24-hour time** — one field that opens a two-column wheel: hours `00`–`23` on the left, minutes `00`–`59` on the right, both scrollable, with **No time** and **Done** below. Never AM/PM, whatever your browser's locale.
- **Recurring tasks** — a *Weekly* repeat option for standing meetings, plus a list of all standing commitments.
- **Today + Tomorrow** — today's tasks sit above the calendar, with a toggle that opens tomorrow's list side by side.
- **Three-state tasks** — the box beside each task cycles on click: pale red (not started) → amber `~` (in progress) → green `✓` (done).
- **Week calendar** — Monday to Sunday (weekend at the end), each day listing its tasks with a done/total counter. Each entry puts its urgency and time on one line and the task itself on the next, so narrow days stay readable. Toggle **1 / 2 / 4 weeks** and page back and forth.
- **Black & white** interface throughout.
- **Export / Import** JSON for backup or moving between browsers.
- **Multi-device sync** through a secret GitHub Gist — see below.

## Syncing two devices

Tasks are stored in `localStorage`, which never leaves the browser that wrote it. Chrome Sync does **not**
carry it either: it syncs bookmarks, passwords and tabs, not site data. So the app can keep a copy of your
data in a **secret GitHub Gist** and merge the two devices.

Setup, once per device:

1. Go to `github.com/settings/tokens` → **Tokens (classic)** → **Generate new token (classic)**.
2. Tick only the **`gist`** scope, give it a long expiry, generate, and copy the token.
3. In the app press **Sync setup**, paste the token, and press **Save & sync**.
   The first device creates the Gist and shows its **Gist ID**.
4. On the second device, paste the same token **and** that Gist ID, then **Save & sync**.

Tick **Sync automatically** and each device syncs when you open the app and a couple of seconds after any change.
Otherwise press **Sync** when you want to push and pull.

**How conflicts resolve:** the merge is per task, not whole-file. Every task and project carries a timestamp,
and the newer edit wins for that item alone — so ticking something off on the laptop and adding a task on the
desktop both survive. Deletions travel as tombstones, so a deleted task does not come back from the other device.

**What to know about the token:**

- It is stored in that browser's `localStorage`, like your tasks. Anyone with access to the device's browser
  profile can read it, so use **Forget token** on a shared or borrowed machine.
- A `gist`-scoped token can read and write *all* your gists and nothing else — no repositories, no account settings.
- A secret gist is not listed or indexed, but it is not encrypted: anyone with its URL can read it.
  Fine for meeting titles; don't put confidential detail in task names.
- Opening the app from a `file://` path may have the browser block the GitHub request. Use the GitHub Pages
  URL (or any `http(s)` host) for syncing.

## Putting it on GitHub Pages

1. Create a repository and push these files.
2. Repo **Settings → Pages → Source: Deploy from a branch**, branch `main`, folder `/ (root)`.
3. Your app is live at `https://<your-username>.github.io/<repo-name>/`.

```bash
git init
git add .
git commit -m "TaskDeck: single-file task manager"
git branch -M main
git remote add origin https://github.com/<your-username>/<repo-name>.git
git push -u origin main
```

> Note: GitHub Pages is public. The tasks themselves are never uploaded — they live only in your browser — but anyone with the URL can use the app.
