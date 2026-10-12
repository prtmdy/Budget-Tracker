# 💰 Budget Tracker

A self-contained expense dashboard that stores its data as an **encrypted Excel workbook in a GitHub repo** you control — not in browser storage — so you can open it from any device and see the same data, as long as you have your GitHub token and passphrase.

## Features

### Storage & sync
- 🔗 **GitHub-backed storage, connect on your terms** — click **🔗 Connect to GitHub** whenever you want to load/edit real data; browsing cached data needs no connection.
- 📗 **Real Excel structure** internally (one sheet per month per year, a Dashboard summary, a Meta sheet for settings) — the same format the **Export Excel** button produces.
- 🔒 **Client-side AES-256 encryption** — your data is encrypted with your passphrase before it's ever sent to GitHub. The repo can stay public; the committed file is unreadable without the passphrase, which is never sent to GitHub or stored anywhere.
- 🔄 **Auto-sync** after every change, serialized through a queue so rapid back-to-back changes never race each other's file version.
- 🔔 **Notifications panel** — last 5 sync attempts (pass/fail/pending) plus recurring-bill reminders, all in one bell-icon dropdown.

### Views
- 📊 **Dashboard**, 📈 **Compare Years**, 📋 **All Expenses**, 🔎 **Trends**, ⚙ **Settings**

### Intelligence
- 💰 **Per-category budgets** — optional monthly limit per category, with progress bars on the Month page
- 🔮 **Spend forecasting** — a "Projected Month-End" KPI when viewing the real current calendar month, extrapolated from your pace so far
- ⚠️ **Anomaly flags** — a ⚠️ badge on transactions notably larger than that description's historical average
- 🔔 **Automatic bill reminders** — fully dynamic (nothing hardcoded): any description appearing in 3+ distinct months is treated as recurring, with its typical day-of-month learned from your actual history. Checked once per session against the real calendar date; reminds you 2 days before, 1 day before, and on the day, if that bill hasn't been logged yet that month.
- 🔎 **Trends tab** — pick a month, see it compared to the one before: category chart, 6-month trend line, and plain-language insight cards (biggest movers by category and by individual description, top category, payment-mix shift, budget status, biggest single expense)

### Speed & usability
- ⚡ **Quick fill** — chips above the Add Expense form that fill category, description, value and mode in one click. A chip appears only for an item logged at the **same value in (nearly) every month, whichever day it's paid** — e.g. Flowers ₹30, Waste Removal ₹150, House Help ₹5,000. "Nearly every" means every month in your last 6 completed months, allowing one miss once you have 5+ months of history; the in-progress month never counts, and each item shows its single most common value. Irregular items (Vegetables, Milk, JioFiber…) are left out. You can also **pin** templates in Settings so they always show; Waste Removal ₹150 and Flat Maintenance ₹500 are pinned by default.
- ⏰ **Custom reminders** — add your own in Settings (or via **＋ Reminder** in the 🔔 panel): a name, an optional amount, and either *every month on day N* or *one time on a date*. You're notified **2 days before, 1 day before, and on the day**, checked whenever you open the app. Days that don't exist in a month (31st in a 30-day month, 30th in February) fall on the month's last day; reminders can be paused or deleted; they sync like everything else. Each reminder shows once per session.
- 🤖 **Smarter category suggestion** — typing a known description auto-selects its usual category
- ⚡ **Global Quick Add** — floating + button on every page, logs into whatever year/month the date falls in
- 🔍 **Search everywhere** — top-bar search across every year, jumps straight to the matching month
- ↩️ **Undo toast** — deleting shows a 5-second Undo; the sync is delayed until that window passes, so undo never touches GitHub
- ☑️ **Bulk actions** — multi-select in All Expenses to re-categorize or delete together
- 🪄 **Better empty states** — actionable messages (clear filters / add here / quick add) instead of bare "no data" text
- 💱 **Multi-currency display** — INR/USD/EUR/GBP/JPY/AUD/CAD/SGD in Settings; updates every amount and chart instantly (display only, no live FX conversion)
- 🎨 **Icon glyphs throughout** — every category has a flat icon in its accent color (badges, legends, dropdowns, Settings), and every *description* gets its own glyph too (🌸 Flowers, 🥛 Milk, 🥦 Vegetables, 💊 Medicine, ⛽ Petrol, 💡 JioFiber…) shown alongside the text in tables, quick-fill chips, search results, and Settings. Descriptions are matched by keyword; anything unrecognized falls back to its category's icon. Chart axes stay plain text on purpose, so dense charts stay readable.
- 💡 **Utilities suggestions** — Electricity ⚡, Gas 🔥, Flat Maintenance 🏢 and JioFiber 💡 are built-in suggested descriptions for the Utilities category. If your synced data predates them, they're added once automatically on your next connect (recorded in the workbook's Meta sheet, so deleting one later sticks).
- ⚙ **Fully customizable categories** — add/remove categories (with color picker and icon) and their suggested descriptions, all synced like any other data

## Your data
The app ships pre-loaded with your real tracked history: **369 transactions, March–October 2026, ₹381,100.18 total** (imported from your own exported workbook, plus the monthly additions below). This is the *starter/fallback* data baked into `index.html` — what a brand-new, not-yet-connected session shows. Your actual source of truth is whatever's synced in your GitHub repo once connected.

**Monthly additions:** Waste Removal ₹150 (Cash, Utilities) was missing in September and October 2026, so it was added on the 4th of those months, and Flat Maintenance ₹500 (Online, Utilities) was added on the 4th of every tracked month, March–October. For data already synced to GitHub, this is applied **once, automatically, the next time you connect** — only to months that already have expenses, never with a future date, never duplicating an existing entry, and recorded in the workbook so deleting an entry later sticks. A sync notification states exactly what was added.

A few naming inconsistencies were found and cleaned up in this data (same items filed under different spellings, one mis-categorized entry, a typo) — amounts were never touched, only labels: Flower→Flowers, Eggs→Egg, Chicken+Egg/Chicken+Eggs→Egg+Chicken, Swwets+Dhokla→Sweets+Dhokla, Fish , Eggs→Fish+Eggs, a stray Utilities "Sweets" entry moved to Food, Doctor→Doctor's Fees, and two Misc "Food" entries moved to Food/"Dining Out".

## Categories (default — fully customizable in Settings)
Food · Utilities · Healthcare · Rent · Misc · Transport · Shopping · Entertainment · Pujo · Travel

## How the "connected" vs "not connected" states work
- **Never connected on this browser**: shows the built-in starter data described above.
- **Connected before, not right now**: shows a cached copy of your last successfully loaded/synced data.
- **Connected**: every change (including Settings — budgets, currency, categories) syncs to GitHub automatically.

Your GitHub token and passphrase are never cached — only the resulting data is.

## One-time GitHub setup
1. **Create (or pick) a repo** for your data.
2. **Generate a fine-grained Personal Access Token**: GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token. Repository access: only this one repo. Permissions → Contents: **Read and write**, nothing else.
3. **Pick a passphrase** for encryption — save it in a password manager. Lost passphrase = unrecoverable data.
4. Click **🔗 Connect to GitHub**, enter Token / Repository (`owner/name`) / Branch / File Path (default `data/budget-data.xlsx`) / Passphrase → **Connect & Load**.
5. First time, no file exists yet — make any change (or click Sync) to create it for real.

## Why your data isn't fully private just from using a token
Reading a file from a **public** repo needs no authentication — the token is only for *writing*. The real protection is the **client-side encryption**. Want the file itself inaccessible too? That needs a private repo (GitHub Pro/Team/Enterprise for private Pages).

## Syncing across devices & conflicts
Auto-sync fires after every change while connected. If GitHub rejects a sync because the file changed elsewhere since you last loaded it, the app logs the conflict in the notifications panel and asks whether to reload the latest version (discarding your local unsynced edit) or cancel and retry later.

## Excel export/edit/re-import
Export, edit the spreadsheet directly, re-import. **Replace** mode makes a month's data match the file exactly (no stale rows, no duplicates); **Merge** mode adds only new rows. Tested end-to-end, including idempotency (reimporting an unchanged export is a true no-op).

## Local usage
Double-click `index.html` — works without hosting, since all it needs is a browser and a connection to `api.github.com`.

## How to host on GitHub Pages
1. Upload `index.html` and `README.md` to your repo root
2. Settings → Pages → Source: `main` branch, `/ (root)` folder → Save
3. Live at `https://<your-username>.github.io/<repo-name>/`
