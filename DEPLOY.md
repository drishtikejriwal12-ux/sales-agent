# Putting Sales Desk online (Vercel)

Vercel doesn't keep files between requests, so the app's data lives in a free hosted database called Turso. The Anthropic API key is optional at first: without it everything works except the chat assistant, which says it isn't connected yet.

## 1. Create the database (Turso, about 3 minutes)
1. Go to turso.tech and sign up (you can use your GitHub login).
2. Create a database. Name it `sales-desk`; pick the Mumbai (or nearest) location.
3. On the database page, copy the **URL** (starts with `libsql://`).
4. Click **Create token** (or "Generate token"), choose full access, and copy it. You only see it once.

Keep these two values for step 2. They are passwords: never put them in a file on GitHub.

## 2. Create the app on Vercel
1. Go to vercel.com, sign in with GitHub, click **Add New > Project**, and import your `sales-desk` repo.
2. Framework preset: **Other**. Leave the build settings empty.
3. Open **Environment Variables** and add:
   - `TURSO_DATABASE_URL`: the URL from step 1.
   - `TURSO_AUTH_TOKEN`: the token from step 1.
   - `SEED_PASSWORD`: a strong password (12+ characters). The first logins use it.
   - `COMPANY_NAME`: your company's name.
4. Click **Deploy**. When it finishes, open the `https://sales-desk-xxxx.vercel.app` link.

## 3. First login and setup
1. Log in as `darsh@salesdesk.local` with your `SEED_PASSWORD`. (The first load after a deploy takes a few seconds while it sets up the database.)
2. Numbers tab > Team: add a real login for yourself and each salesperson, then deactivate the two demo salespeople (Aman, Neha) and, once your own login works, `darsh@salesdesk.local`.
3. Leads tab: open each lead and set its owner.
4. Numbers tab > Price list: add your mesh and AAC prices with purchase cost and GST. Quotes stay blocked until this is done.
5. On phones: open the link, then "Add to Home Screen".

## Later: turn on the chat assistant
1. Go to console.anthropic.com, sign in with your company email, add billing, then Settings > API keys > Create key. Copy it (starts with `sk-ant-`).
2. In Vercel: your project > Settings > Environment Variables > add `ANTHROPIC_API_KEY` with the key.
3. Deployments tab > the latest one > **Redeploy**. Chat now works; your data is kept (it's in Turso).

## Updating later
Upload changed files to the GitHub repo; Vercel redeploys automatically.

## Costs and limits
- Vercel's free Hobby plan is for personal, non-commercial use. For a company app, Vercel expects the Pro plan (about $20 per month per team member who manages the project; salespeople using the app don't count).
- Turso's free plan is more than enough for a small sales team.
- A chat reply can take up to 60 seconds before Vercel cuts it off; normal replies take a few seconds.
- Claude usage is billed by Anthropic per message. The Numbers tab shows tokens used over the last 30 days; console.anthropic.com shows spend and lets you set a monthly limit.
