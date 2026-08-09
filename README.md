# Ledger — a private expense tracker for your phone

Everything lives on your device. No accounts, no backend, no App Store.

## What's in this folder
- `index.html` — the whole app (structure, styling, logic)
- `manifest.json`, `sw.js` — make it installable on your home screen and work offline
- `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` — app icons

## How it works
- **Storage:** everything (entries, receipt photos, budgets, recurring items) is saved in IndexedDB, a database built into your phone's browser. Nothing leaves your device except the one API call described below.
- **Manual logging:** tap category → numpad → save.
- **Photo logging:** snap a receipt, it's sent to Claude to extract merchant/amount/date/category, you confirm, it saves — and the photo itself is kept in the Receipts archive.

## Getting it onto your iPhone
This needs to be hosted somewhere with `https://` for the install and offline features to work fully (a plain local file will open, but Safari won't offer "Add to Home Screen" reliably for `file://` URLs). The easiest free option:

1. Push this folder to a GitHub repository.
2. In the repo settings, enable **GitHub Pages** for that repository (serves it at `https://yourname.github.io/reponame/`).
3. Open that URL in Safari on your iPhone.
4. Tap the Share icon → **Add to Home Screen**.

If you'd like, I can walk you through setting up the GitHub repo and Pages next — that's a good next step to do in Claude Code, since it involves git commands.

## Adding your API key (for receipt photo reading)
Open the app → tap the **☰** menu → **Settings** → paste in your Anthropic API key. Get one at https://console.anthropic.com (pay-as-you-go, receipt parsing costs a small fraction of a cent per photo).

**Important tradeoff to know about:** because this app has no backend server, the photo is sent *directly from your phone's browser* to Anthropic's API, using your key. That requires a special header (`anthropic-dangerous-direct-browser-access`) that Anthropic provides specifically for this kind of use — but it does mean your API key sits in the app on your phone (in IndexedDB) rather than hidden on a server. For a private app only you use, this is a reasonable tradeoff. If you ever wanted to harden it further, the next step would be a tiny serverless function that holds the key instead — happy to help build that later if it matters to you.

You can skip the API key entirely and just use "Enter manually" after snapping a photo — the photo still gets archived, you just type in the details yourself.

## Data policy on the API call
Per Anthropic's current API terms: inputs/outputs are auto-deleted from their backend within 30 days, and are not used for model training by default. Only the single receipt image + a short instruction is sent — nothing else about your other expenses or data.

## What's not in v1 (by design, to keep this shippable)
Investments tracking, multi-user accounts, search/export, data sync across devices. All good candidates for later.
