# Sales Desk

A phone-friendly web app where sales managers and salespeople chat with a junior sales assistant. It carries over the junior-sales agent (leads, WhatsApp, email, calls, morning briefing, performance, end-of-day review, scheduling, approvals) and keeps records in a database instead of spreadsheet files.

## Run it

```
npm install
cp .env.example .env    # add ANTHROPIC_API_KEY for the chat
npm start               # http://localhost:3000
```

On first run it loads the 13 leads and 5 first-round drafts from `data/seed.json` (exported from the project's `sales/` records by `scripts/export_seed.py`) and creates three demo logins, all with the password in `SEED_PASSWORD` (default `demo1234`):

| Login | Role | Sees |
|---|---|---|
| darsh@salesdesk.local | Manager | Whole team, approvals, prices, targets, logins |
| aman@salesdesk.local | Salesperson | P001, P002, P003 |
| neha@salesdesk.local | Salesperson | P005, P007 |

The other eight leads are unassigned; a manager assigns them from the lead screen.

## Rules the code enforces

These live in `src/desk.js`, which both the screens and the assistant's tools call, so a chat message can't talk its way around them.

- The assistant has no tool that sends, approves or sets prices. Drafts go to the approval queue; only a manager decides, on the Approvals screen.
- Approved is not sent. A salesperson sends from their own phone or mail, then taps "I've sent it", which logs the touch and sets the day 3 / 7 / 14 follow-up.
- WhatsApp drafts under 60 words, emails under 120 with a subject under 9. A draft that mentions a price is refused unless the lead has an approved quote.
- Quotes come only from the manager's price list; a missing price, GST rate or purchase cost blocks the quote. Lines below cost are flagged.
- A wa.me link is built only for a phone number already on record, and a phone or email can only be recorded with its public source.
- A salesperson sees and changes only their own leads. Marking a deal Lost, or parking a High lead, goes to a manager.

## Layout

| Path | What |
|---|---|
| `src/desk.js` | Business rules and queries |
| `src/tools.js` | The assistant's tools (wrap `desk.js`) |
| `src/agent.js` | System prompt and the Claude tool loop (Opus 5.5, adaptive thinking, server-side refusal fallback) |
| `src/server.js` | Express API and chat endpoint |
| `src/db.js`, `src/auth.js`, `src/seed.js` | SQLite schema, logins, first-run data |
| `public/` | The web app (plain JS, no build step) |
| `test/` | `npm test`: rules, role scoping, and the agent loop against a scripted API |

## Hosting

Any Node host with a persistent disk works (Render, Railway, a small VPS). Set `ANTHROPIC_API_KEY`, `DB_PATH` on the persistent disk, `NODE_ENV=production`, and change `SEED_PASSWORD` or reset the demo passwords before inviting the team. A `Dockerfile` is included.
