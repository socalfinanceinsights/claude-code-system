# MAINTENANCE.md

This document covers how to maintain this public repo — specifically what was scrubbed before publishing, the sync process from the private working version, and what this repo is and isn't.

---

## 1. PII and Credentials Policy

The following categories of real data were replaced before any files were committed to this repo. They must stay replaced.

**What was scrubbed:**

| Type | Replaced with |
|---|---|
| Real email addresses | `your.email@example.com` |
| Phone numbers | `(555) 555-5555` |
| Personal names | `YOUR_NAME` |
| Booking / scheduling URLs | `YOUR_BOOKING_URL` |
| API keys and tokens | `YOUR_API_KEY_HERE` |
| Business domain names in filter arrays | Removed or genericized |

**Rule: never commit any of the following to this repo:**
- Real email addresses (yours or anyone else's)
- Phone numbers
- Full names tied to real individuals
- API keys, OAuth tokens, or any credential material
- Spreadsheet IDs that point to real private data

**What the `.gitignore` excludes:**
- `*.html` (sidebar UIs contain project-specific structure)
- `*.xml`
- `token.json`, `credentials.json`, `creds*.json`
- `.env`
- `__pycache__/`, `*.pyc`
- `node_modules/`

If you fork this and add project-specific files, audit your `.gitignore` before pushing.

---

## 2. Sync Process

The private AG_Work workspace is the source of truth. This public repo is a sanitized mirror — changes flow one direction: private to public.

**Steps to sync an update:**

1. Make the change in the private AG_Work version first. Test it there.
2. Copy the changed file(s) to the corresponding path under `public_repo/claude-os-master/`.
3. Run a PII scan before staging anything:
   - Search the changed files for your real email, phone number, name, and any API keys.
   - Grep patterns to check:
     - Your actual email domain (e.g., `yourdomain.com`)
     - Your actual phone number digits
     - Your actual name
     - Any string matching: `[A-Za-z0-9]{20,}` assigned to a variable named `key`, `secret`, `token`, `apiKey`, or similar
4. Replace any PII found with the standard placeholders listed in Section 1.
5. Stage specific files explicitly — never `git add -A` blindly. Review the diff before committing.
6. Write a clear commit message describing what changed and why.
7. Push to `master`.

**Never skip the PII scan.** The diff between what works privately and what's safe publicly is exactly the kind of thing that slips through when you're moving fast.

---

## 3. What This Repo Is (and Isn't)

**What it is:**

A showcase of real production architecture, sanitized for public viewing. The scripts in `projects/` reflect actual automation built for an executive search practice — LinkedIn post ingestion, market intelligence pipelines, BD contact enrichment, candidate screening. The `.claude/` directory reflects an actual Claude Code operating system used in production.

**What it isn't:**

- A deployable package. You cannot clone this and run it. There is no setup script, no bundled credentials, no pre-configured sheet structure.
- A supported open-source project. No issues, no PRs, no support.

**What you'll need to adapt if you use this as a reference:**

- **Script Properties:** Every `PropertiesService.getScriptProperties().getProperty(...)` call in the `.gs` files references a key you'll need to populate in your own GCP/Apps Script project's Script Properties. The key names are visible in the code; the values are not.
- **Sheet IDs:** References to spreadsheet IDs are genericized or removed. You'll need to wire your own sheets.
- **Spreadsheet structure:** Column layouts, tab names, and trigger schedules are specific to this implementation. Treat them as a reference pattern, not a spec to copy verbatim.
- **Claude skills and agents:** The `.claude/` system works with Claude Code CLI. The skills and agent profiles are designed for a specific workflow — read them for architectural ideas, then adapt to your own context in `CLAUDE.md`.

**The intended audience** is someone who already knows Google Apps Script, Python, and Claude Code, and wants to see how a production system is structured. It is reference material.
