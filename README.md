# Gooby Asset Tracker — Deployment Guide

## File Structure
```
gooby-asset-tracker/
├── index.html              ← main app
├── wrangler.toml           ← Cloudflare config
└── functions/
    └── api/
        └── data.js         ← KV API (GET/POST)
```

---

## Step 1 — GitHub

1. Go to github.com → **New repository**
2. Name: `gooby-asset-tracker` → Create
3. Upload all 3 files keeping the folder structure exactly as above

---

## Step 2 — Create KV Namespace in Cloudflare

1. Go to **dash.cloudflare.com** → log in
2. Left menu → **Workers & Pages** → **KV**
3. Click **Create a namespace**
4. Name: `ASSET_TRACKER_KV` → Add
5. **Copy the ID** shown next to the namespace (looks like `abc123def456...`)

---

## Step 3 — Deploy with Cloudflare Pages

1. Left menu → **Workers & Pages** → **Create** → **Pages** tab
2. **Connect to Git** → authorize GitHub → select `gooby-asset-tracker`
3. Set build settings:
   - Framework preset: **None**
   - Build command: *(leave blank)*
   - Build output directory: `/`
4. Click **Save and Deploy**
5. Wait ~1 min for first deploy

---

## Step 4 — Bind KV to your Pages project

This is the critical step that connects the API to the database.

1. Go to your Pages project → **Settings** tab
2. Left: **Functions** → scroll to **KV namespace bindings**
3. Click **Add binding**:
   - Variable name: `ASSET_TRACKER_KV`
   - KV namespace: select `ASSET_TRACKER_KV` from dropdown
4. Click **Save**
5. Go to **Deployments** tab → click **Retry deployment** (or push a new commit)

---

## Done!

Your app is now live at:
`https://gooby-asset-tracker.pages.dev`

- ✅ All team members share the **same data** in real-time
- ✅ Auto-saves to Cloudflare KV on every change
- ✅ Polls for remote updates every 30 seconds
- ✅ Falls back to localStorage if offline

---

## Updating the app

Push any change to GitHub → Cloudflare auto-redeploys in ~1 minute.

```bash
git add .
git commit -m "update"
git push
```
