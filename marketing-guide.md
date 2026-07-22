# **Marketing Guideline**

This document defines the required marketing workflow for all projects under the seanslab stack. The standard mirrors the Engineering Guideline's "firmware bring-up discipline," adapted for marketing: **design first, review before acting, log every action, verify every action landed, and measure real results before declaring success.**

Marketing work is **result-oriented**: the goal is a metric moved, not content shipped. But results only become repeatable through detail and process — never confuse activity (posts sent, emails delivered) with outcomes (followers gained, signups, replies, revenue). Both matter; only outcomes count as "done."

Companion documents: `Engineering Guide.md` (workflow DNA), the internal Marketing Guidelines (brand + agent conduct), and the internal Program Manager Guidelines (management style).

---

## **0) Workspace and Project Structure**

1. **Root work folder (authoritative):** ROOTDIR — project by project, same as engineering.

2. **Every marketing project must have a GitLab repo — no exceptions.**

    - Hosted at **`gitlab.jing.zone`** (self-hosted GitLab).
    - For marketing projects, this supersedes the Engineering Guideline's GitHub-repo rule.
    - New project setup:
        - Create a new folder under ROOTDIR
        - Initialize git
        - Create the repo on `gitlab.jing.zone` and configure it as the remote
        - First commit = the project skeleton below, before any campaign work starts

3. **Standard project skeleton:**

    ```
    <project>/
    ├── design/          # MDDs (Marketing Design Documents), one per campaign
    ├── content/         # drafts, copy, images, decks — versioned before publishing
    ├── logs/            # mkt-log-YYYYMMDD.md — every action, every day
    ├── results/         # result reviews, metric snapshots, screenshots/evidence
    ├── requirements/    # req-YYYY-Www.md — one per ISO week (Section 6)
    └── tasks/           # todo.md, lessons.md
    ```

4. **Commit and push periodically.** Content must be committed to the repo **before** it is published anywhere — a published post whose source exists only in a chat window or clipboard is an unrecoverable state (same principle as archiving firmware before flashing).

5. **Privacy boundary:** recruiting/outreach PII, private-contact material, and anything designated local-only **never gets committed or pushed** — even to the self-hosted GitLab. When in doubt, keep it local and gitignored.

---

## **1) The Five-Stage Workflow**

Every campaign moves through five gates, in order. No stage may be skipped; a failed gate sends the work back, not forward.

```
Design → Design Review → Action → Action Review → Result Review
```

### **Stage 1 — Design (no blind execution)**

Start with a written **MDD (Marketing Design Document)** before any meaningful action — the marketing equivalent of an SDD (System Design Document). File: `design/mdd-<campaign>-YYYYMMDD.md`. It must include:

- **Objective** — one sentence, one measurable outcome (e.g., "+50 followers," "10 qualified replies," "500 landing-page visits").
- **Audience** — who exactly, and what they currently believe/feel.
- **Message** — the single idea the audience should walk away with, and why they'd care.
- **Channel plan** — which surfaces, what format, what cadence, which account.
- **Assets needed** — copy, images, video, landing pages; who/what produces them.
- **Success metrics + review window** — the numbers that define success, and *when* they'll be read (e.g., 24h / 7d / 30d). A campaign without a review window cannot be result-reviewed and therefore cannot be declared done.
- **Risks + red-line check** — brand risk, platform-rule risk, and an explicit pass over Section 5 (Red Lines).
- **Rollback plan** — can it be deleted/edited after publishing? What happens if it misfires? What's the containment move?

Re-plan immediately when reality diverges: if a channel behaves unexpectedly, a message flops, or platform rules change — stop and revise the MDD. Do not keep pushing a broken campaign.

### **Stage 2 — Design Review (independent, before any action)**

- **Reviewer must be independent of the author** — another person, or a dedicated review agent/pass; never the same thread that wrote the design. Same hard gate as engineering's peer code review.
- Review checklist:
    - **Elegance, from the customer's eye** — does this deliver an elegant sense? Does it fit the brand persona (Solid and Premium)? Would we be proud of this in six months?
    - **Message–market fit** — would the stated audience actually care? Is the objective realistic for the channel and effort?
    - **Metric sanity** — are the success metrics measurable, attributable, and honest (not vanity)?
    - **Red lines** — Section 5 pass, item by item.
    - **Deconfliction** — check the action logs of *all* other active posting/outreach loops before approving; two loops touching the same audience or surface must be explicitly sequenced.
    - **Detail check** — dates, links, names, prices, claims all verified against source.
- Record the review outcome **in the MDD** (reviewer, findings, resolution). Every blocking finding must be resolved before Stage 3. Non-blocking findings may be deferred only with an explicit item in `tasks/todo.md`.

### **Stage 3 — Action (execute per design, log everything)**

- Execute exactly what the reviewed MDD says. Scope discipline: no drive-by posts, no improvised extra channels. If a better idea appears mid-flight, it goes back through Design Review (fast is fine; skipped is not).
- **Before every send/post/outreach:**
    - Check the relevant ledgers and logs (outreach ledger, this project's `mkt-log-*.md`, other loops' action logs) — **never duplicate a touch to the same person or surface.**
    - Commit the final content to the repo.
- **Log every action** in `logs/mkt-log-YYYYMMDD.md` (Section 2) — planned before, outcome after.
- Work in small batches so cause and effect stay isolatable — the marketing equivalent of small commits.

### **Stage 4 — Action Review (verify it landed, same session)**

An action is not done when the button is pressed; it is done when it is **verified live and correct**:

- Open the live post/sent message and confirm: right account, right surface, copy exact, links resolve, images render, formatting intact.
- Capture **evidence** (URL + screenshot) into the day's log and `results/`.
- If anything is wrong: fix, edit, or execute the rollback plan **within the same session**, and log the miss.
- Never mark an action complete without proof — same bar as "never mark done without passing tests."

### **Stage 5 — Result Review (outcomes, not activity)**

At each review window defined in the MDD:

- Pull the **real numbers** (platform analytics, ledger replies, conversions) — never estimate or extrapolate from memory.
- Write `results/result-<campaign>-YYYYMMDD.md`: metrics vs. targets, what worked, what didn't, and an explicit verdict — **Scale / Iterate / Kill.** Flat or unclear results default to Iterate-or-Kill, not to quiet continuation.
- Capture lessons in `tasks/lessons.md` — especially after any misfire or user correction — as a preventative rule, and review relevant lessons at the start of each session.
- The verdict feeds the next MDD. A campaign with no Result Review on file is not finished, no matter how much was posted.

---

## **2) Action Logging (Required, Every Action)**

1. **Every marketing action gets logged** — post, reply, DM, email, ad change, page publish, follow/unfollow batch, escalation. If it touched an external surface, it's in the log. No exceptions.

2. **File name:** `logs/mkt-log-YYYYMMDD.md` (e.g., `mkt-log-20260722.md`) — one file per day per project.

3. **Entry template:**

    ```markdown
    ### HH:MM — <action type> — <channel / account>
    - What: one-line description
    - Campaign: <MDD reference>
    - Pre-check: dedup/ledger/deconflict checked (name what was checked)
    - Content ref: <repo path or commit>
    - Status: planned | posted | sent | failed | rolled-back
    - Evidence: <live URL / screenshot path>
    - Next: follow-up, if any
    ```

4. **Log before and after.** Write the entry with `Status: planned` before acting; update it with the outcome and evidence immediately after. A gap between the two is where duplicates and misfires live.

5. **The logs are the deconfliction layer.** Any agent or loop about to act on a shared surface must read the recent logs first. This is why logging is a hard rule, not bookkeeping: an unlogged action is invisible to every other loop and *will* eventually cause a duplicate or a contradiction in public.

6. If a campaign is active but no action was taken on a given day, one line stating why (e.g., "holding for 7d review window") keeps the record honest.

---

## **3) Brand and Quality Bar**

1. **Demand elegance (balanced).** For all branding and marketing work, think from the customer's eye: does this work deliver an elegant sense? Brand persona: **Solid and Premium.**
2. **Genuinely helpful, not performatively helpful.** No filler, no hype-padding, no half-baked replies on any messaging surface. Every public word either earns attention or spends trust.
3. **Detail is brand.** A wrong date, a broken link, or a sloppy image *is* the brand message that day. The detail checks in Stages 2 and 4 exist because customers see details before they see strategy.
4. **Do things fast, think slow and deeply.** Speed in execution, never in judgment. When uncertain about a public action, escalate with 1–2 recommended options rather than guessing.

---

## **4) Task Management and Reporting**

1. **Plan in `tasks/todo.md`** with checkable items and acceptance criteria; break campaigns down until each sub-job is small enough to execute, verify, and reason about independently. Mark which sub-jobs can run in parallel (asset production usually can; publishing to the same surface usually cannot).
2. **Proactive reporting** — report periodically without waiting to be asked: what was done, what's next, blockers/risks, and verification status (live links, log entries, metric snapshots).
3. **Manage to details** — break down deep, keep the list accurate, mark items complete only after their verification step ran.

---

## **5) Red Lines (Check at Design Review and Before Every Action)**

1. **IP red line** — never disclose proprietary technology, unreleased product details, internal metrics, or partner information without explicit clearance.
2. **PII stays local** — recruiting/outreach personal data is never committed, pushed, or published. Ledgers containing PII live local-only.
3. **Restricted material** — any person, meeting, or material designated local-only/never-public is never referenced in any marketing artifact, in any form, on any surface.
4. **Platform rules** — respect each surface's norms and each account's standing policies (e.g., no public comments where DM-only policy applies; use the designated execution surface for each account).
5. **No duplicate touches** — the ledger/log check before every outreach is mandatory; a duplicate send to the same person is a trust-damaging incident, not a typo.
6. **Authorized autonomy only** — autonomous posting happens only within explicitly granted scopes and caps; everything else gets human sign-off first.

---

## **6) Requirements Capture**

1. **Log every user requirement in the weekly requirements file**

    - **File name: `requirements/req-<ISO-year>-W<ISO-week>.md`** — one file per ISO week (e.g., `req-2026-W30.md` covers Mon Jul 20 – Sun Jul 26, 2026).
    - ISO weeks: Monday-start, ISO year-week pair (`date +%G-W%V`). Note the ISO year can differ from the calendar year at boundaries (e.g., 2025-12-29 falls in `2026-W01`).
    - When the user issues a requirement — a campaign ask, a brand rule, a channel policy, a new red line, a cap change — briefly add it as its own entry.
    - Keep newest on top within the file (prepend above the most recent ID). Requirement IDs (`R-XXXX`) run continuously across weekly files — never reset at a new week.
    - Put the agent's reply, in brief, into the same entry so the decision lives next to the requirement.

2. **Requirements feed the working rules.** The weekly requirements files are the audit trail; a requirement that changes standing policy (a red line, a cap, a channel rule) must also be folded into the relevant section of this guide or the affected MDDs — never left only in the log.

---

## **Summary: Non-Negotiables**

- MDD written first; no action before Design Review passes (reviewer independent of author)
- **GitLab repo on `gitlab.jing.zone` for every marketing project** — content committed before publishing
- **Every action logged in `logs/mkt-log-YYYYMMDD.md`** — planned before, evidence after
- Every action verified live (Action Review) — no proof, not done
- Every campaign closed with a Result Review — real numbers vs. targets, Scale/Iterate/Kill verdict
- Results over activity; detail and process are how results become repeatable
- Ledger/deconflict check before every external touch; no duplicate touches
- Red lines checked twice: at Design Review and again before each action
- Elegance from the customer's eye — Solid and Premium — on everything public
- Lessons captured in `tasks/lessons.md` after every correction or misfire
- Every user requirement logged in the weekly `requirements/req-YYYY-Www.md` (ISO week, newest on top) with the agent's brief reply in the same entry
