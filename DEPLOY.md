# Putting Sales Desk online (Render)

You need: the code in your GitHub repo, a Render account (render.com, sign in with GitHub), and an Anthropic API key.

## 1. Get an Anthropic API key
1. Go to console.anthropic.com and sign in with your company email.
2. Settings > Billing: add a card and some credit.
3. Settings > API keys > Create key. Name it "Sales Desk". Copy it (starts with `sk-ant-`). You only see it once.

Never put the key in a file on GitHub. It only goes into Render (step 2).

## 2. Create the app on Render
1. In Render: New > Blueprint, pick your `sales-desk` repo. Render reads `render.yaml`.
2. It asks for three values:
   - `ANTHROPIC_API_KEY`: paste the key from step 1.
   - `SEED_PASSWORD`: a strong password (12+ characters). The first logins use it.
   - `COMPANY_NAME`: your company's name (the assistant uses it).
3. Click Apply. The first build takes a few minutes. The plan is Starter (paid, a few dollars a month) because the free plan has no disk and would lose your data on every restart.
4. When it shows "Live", open the `https://sales-desk-xxxx.onrender.com` link. That's the app; it works on phones.

## 3. First login and setup
1. Log in as `darsh@salesdesk.local` with your `SEED_PASSWORD`.
2. Numbers tab > Team: add a real login for yourself and each salesperson (temporary password, 8+ characters), then deactivate the two demo salespeople (Aman, Neha) and, once your own login works, the `darsh@salesdesk.local` one.
3. Leads tab: open each lead and set its owner.
4. Numbers tab > Price list: add your Smart Fab mesh and VBS AAC prices with purchase cost and GST. Quotes stay blocked until this is done.
5. On phones: open the link, then "Add to Home Screen" so it behaves like an app.

## Updating later
Upload changed files to the GitHub repo; Render redeploys automatically. Your data is on the disk and is kept.

## Costs to watch
Render Starter plus disk, billed by Render. Claude usage is billed by Anthropic per message; Numbers tab shows tokens used over the last 30 days, and console.anthropic.com shows spend. You can set a monthly spend limit there.
