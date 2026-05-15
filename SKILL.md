---
name: self-performance-review
description: >
  Use this skill whenever Sir wants a weekly self-performance review, dipstick check, retrospective on his own work, or "how am I going" assessment. Trigger on phrases like "self-performance-review", "weekly review", "how did I do this week", "performance check", "dipstick on my week", "what did I ship this week", "where are my blind spots", "review my week", "retro on my week", "audit my work", "honest take on how I'm performing", or any request asking for an outside view on Sir's own output, communication, time allocation, or follow-through. The skill pulls evidence from Slack, Notion, Gmail, Google Calendar, Claude Code sessions, and any meeting transcripts it can find, then benchmarks against established performance and communication frameworks (Grove, Drucker, Bezos, async-comms hygiene) and produces a structured weekly review with blind-spot calls and 3 concrete improvements for next week. Defaults to the last 7 days; accepts a custom window. Reports are written to ~/Documents/Claude/performance-reviews/YYYY-MM-DD.md so each week's run can compare against the prior week's improvement targets.
---

# Self Performance Review

A weekly dipstick on Sir's actual work, communication, and decisions — measured against published performance and communication best practice, not vibes.

Output is a single structured report. Honest, opinionated, evidence-backed. Praise that's earned. Criticism that's specific. No hedging, no horoscope language, no "you're doing great!" filler.

## When to invoke

- Sir runs `/self-performance-review` (or any of the trigger phrases in the description)
- Default lookback: **last 7 days** (Mon-Sun if today is Mon/Tue, otherwise rolling 7)
- Sir can override with "last 2 weeks", "last month", "since Friday", etc.

## The frameworks the review anchors to

These are the yardsticks. Cite them by name in the report so Sir can audit the logic.

### Performance

- **Grove — High Output Management.** Output = activity × leverage. Activity is busywork; output is *what shipped that affects the system*. Measure shipped artifacts, decisions made, people unblocked — not hours logged or messages sent.
- **Drucker — The Effective Executive.** Effectiveness = doing the right things, not doing things right. First things first; concentration on a few priorities; decisions over reactions.
- **Bezos — Type-1 vs Type-2 decisions.** Irreversible (Type-1) decisions deserve careful analysis; reversible (Type-2) decisions should be made fast. Spending Type-1 energy on Type-2 work is a leak. Spending Type-2 speed on Type-1 work is reckless.
- **Reforge / Lenny — leverage and compounding.** Did the week's work compound (skills, systems, brand, code, relationships) or evaporate (tickets closed but nothing accrued)?

### Communication

- **Async-first hygiene.** Decisions documented; context lives where it's needed; written clarity > meeting volume.
- **Response latency.** Are threads being left hanging? Soft commitments turning into ghosting?
- **Tone calibration.** Curt, sycophantic, clear, or evasive — sampled from actual outbound messages.
- **Ratio of asks vs answers, gives vs takes.** Is Sir mostly extracting or mostly contributing in his core channels?

### Meeting hygiene (Grove + Cal Newport — deep work)

- Total meeting hours / week
- Back-to-back density (any 3-hour-plus blocks of solid meetings?)
- Recurring vs ad-hoc split
- No-shows and reschedules (his and theirs)
- Deep-work blocks preserved (any 2+ hour uninterrupted blocks?)

## The data pull — sources and tools

Run all of these **in parallel** at the start of the review. Note any source that errors or returns empty so the report can flag the gap.

### Required pulls

| Source | Tool | What to extract |
|---|---|---|
| **Calendar** | `mcp__3dd3d3ce-a539-4928-bc84-51b139526c42__list_events` | Every event in the window. Count, total hours, recurring vs ad-hoc, meeting-free blocks. Inspect descriptions for Read.ai/Fathom/Granola transcript links. |
| **Gmail** | `mcp__68f17ef3-ef45-45c7-94ec-dc28b9908980__search_threads` | Sir's outbound threads in window (`from:me after:YYYY/MM/DD`). Count, top recipients, threads where Sir hasn't replied (`is:unread to:me`). Also look for unreplied threads from key people. |
| **Slack** | `mcp__55031377-b138-41eb-bb4a-2b0e7730a3ec__slack_search_public_and_private` | Sir's posts in window (`from:@justin after:YYYY-MM-DD`). DMs he sent, channels he was active in, threads where he was @-mentioned but didn't reply. |
| **Notion** | `mcp__b8c48844-29ce-4b13-9e29-e12205bf18c1__notion-search` + `notion-query-meeting-notes` | Pages he created/edited in window. Meeting notes with action items assigned to him. |
| **Claude Code sessions** | `mcp__ccd_session_mgmt__list_sessions` + `mcp__ccd_session_mgmt__search_session_transcripts` | Sessions in window. Topic clusters, what shipped, recurring problems (same issue across multiple sessions = signal). |
| **Git / shipped work** | Bash | For Sir's active repos — `gh pr list --author @me --state merged --search "merged:>YYYY-MM-DD"` across the key repos (get-orbit, orbit-for-claude, sophiie-voice-writer, orion-by-orbit, orbit-dictation, sophiie-ai/monorepo if accessible). Plus tags pushed for Sparkle apps. |

### Optional / nice-to-have

- **Transcripts** — if any Calendar events contain Read.ai/Fathom/Granola/Otter links, note them in the report but don't try to fetch the content (most live behind auth). Just count: "8 meetings, 5 had transcripts attached."
- **Aircall** — Sophiie's calls. If transcripts surface via Notion or Slack, include. Skip otherwise.

### Hat-tagging

Every piece of work gets tagged **[Sophiie]**, **[Orbit]**, or **[Personal]** in the report. They have different success criteria. Don't conflate.

- **Sophiie work** = AI workflow systems, trades-industry, monorepo, Braze/Sophiie campaigns, anything in the sophiie-ai org or sophiie.ai domain
- **Orbit work** = get-orbit, orbit-for-claude, orion-by-orbit, orbit-dictation/Comet, yourorbit.team domain, anything customer-facing for Orbit
- **Personal** = Claude config, personal tooling, learning, side projects, family

## The report structure

Write to `~/Documents/Claude/performance-reviews/YYYY-MM-DD.md` and also print to chat. Use today's date as the filename.

If a prior report exists in that directory, **read the most recent one first** and explicitly check: did Sir hit the 3 improvement targets from last week? Call it out in section 7.

### Report template

```markdown
# Weekly Self-Review — {window-start} to {window-end}

**Hats this week:** Sophiie {X%} · Orbit {Y%} · Personal {Z%}  *(rough split by hours/output)*

---

## 1. Headline verdict — one paragraph

Two to four sentences. Honest. What kind of week was it: shipping, planning, scattered, deep, reactive, strategic? Lead with the verdict, not the qualifiers.

## 2. What shipped — the output ledger

Grove's test: *did the system change?*

- **[Hat]** Concrete shipped artifact — PR merged, tag cut, doc published, decision made, person unblocked. One line each. Link where possible.
- Aim for 5-15 items. If under 5, that's a signal — flag it.
- Separate **shipped** (in the wild) from **in-flight** (started, not landed).

## 3. Time allocation — where the week went

| Bucket | Hours | % of working hours | Verdict |
|---|---|---|---|
| Meetings (synchronous) | | | |
| Deep work (Claude sessions, coding, writing) | | | |
| Async comms (Slack, email) | | | |
| Unaccounted | | | |

**Meeting density:** {X} meetings, longest back-to-back stretch {Y} hours, {Z} meeting-free 2hr+ blocks.

**Verdict:** ratio of meetings to ship — healthy / heavy / starved.

## 4. Communication audit

### Outbound volume + cadence
- Slack messages sent: {N}. Top 3 channels.
- Email threads initiated by Sir: {N}. Top 3 recipients.
- Average response latency to people who messaged him first: {best estimate}.

### Tone sample
Pull 2-3 actual sentences Sir wrote this week. Don't anonymise — name the channel/recipient. Audit against the Slack voice memory: hyphens, dropped apostrophes, no "Cheers/Best", soft hedges as collaboration signals. Note where the actual voice matched, and where it drifted (too curt, too formal, sycophantic, hedging when he should have committed).

### Asks vs answers
Rough ratio of "Sir asked someone for something" vs "Sir answered someone's ask". Heavy extraction = warning sign.

## 5. Decisions made — Type-1 vs Type-2

- **Type-1 (irreversible)** decisions this week: list them. Each with a one-line "was this analysed with appropriate care?" verdict.
- **Type-2 (reversible)** decisions this week: list them. Each with a one-line "was this made fast enough?" verdict.
- **Decisions deferred:** anything Sir punted on that's costing optionality. Name them.

## 6. Blind spots — the uncomfortable section

This is the section worth running the skill for. Be specific. Cite evidence.

- **Unanswered threads.** People waiting on Sir. Name them, name the channel, name how long.
- **Dropped commitments.** "I'll get back to you on X" said and not delivered. Cross-reference Slack/email/Notion action items.
- **Repeated patterns.** Same problem surfacing across multiple Claude sessions, multiple Slack threads, multiple meetings. Likely a structural issue, not a one-off.
- **Topic avoidance.** Anything Sir has deflected three+ times. Name it.
- **Energy leaks.** Where Type-1 care was spent on Type-2 work, or vice versa.
- **Compounding gaps.** Things that didn't compound — pure ticket-closing, redo work, busywork that didn't build the system, the brand, or skills.

## 7. Last week's improvement targets — did Sir hit them?

If a prior report exists, list the 3 targets verbatim and grade each:
- ✅ Hit — with evidence
- ⚠️ Partial — what was done, what wasn't
- ❌ Missed — why, and is it still worth pursuing

If no prior report, skip this section and write: *(First review — no prior targets to grade.)*

## 8. Three improvements for next week

Three. Not five. Not ten. Three.

Each one must be:
- **Specific** — "respond to Tom's thread in #orbit-feedback by Tuesday" not "communicate better"
- **Measurable** — Sir or the next review can verify it
- **Leverage-bearing** — fixes something that compounds, not a one-off

Format:
1. **{Target}** — Why: {one-line}. How to verify: {one-line}.
2. **{Target}** — Why: {one-line}. How to verify: {one-line}.
3. **{Target}** — Why: {one-line}. How to verify: {one-line}.

## 9. Earned praise (only when earned)

One short paragraph. The genuinely strong piece of the week — the thing that, if repeated, would make next quarter materially better. If nothing earns praise this week, write *(Nothing landed cleanly enough to call out. Next week.)* and move on. Do NOT manufacture praise.

---

*Frameworks referenced: Grove (High Output Management), Drucker (The Effective Executive), Bezos (Type-1/Type-2 decisions), async-first communication hygiene.*
```

## Behaviour rules

1. **Be specific. Always cite evidence.** "You ghosted Tom on Monday" not "communication could improve". Pull the actual thread, the actual date.
2. **Never hedge to spare feelings.** Sir asked for blind spots. Surface them. Caldwell voice is fine here — measured, direct, occasionally blunt.
3. **No fake praise.** If section 9 has nothing real, leave it empty with the placeholder. Manufactured praise debases real praise.
4. **No generic advice.** "Improve focus" / "be more proactive" / "communicate clearly" are banned. Every recommendation names a person, a thread, a repo, or a decision.
5. **Hat-tag everything.** Sophiie, Orbit, Personal. Different success criteria. Conflating them is the most common review-tool failure.
6. **One report per run.** Write the markdown file, print the report. Don't ask follow-up questions mid-report — make the calls and let Sir push back after.
7. **If data is missing, say so.** "No Aircall transcripts accessible this run" is fine. Hallucinating data is not.
8. **Compare to last week if a prior report exists.** Section 7 is load-bearing. Without it, weekly reviews become disconnected snapshots.

## Failure modes to actively avoid

- **The horoscope.** Vague, applies to anyone, no specific evidence. → Banned.
- **The participation trophy.** "Great week overall!" with no shipped output to back it. → Banned.
- **The pile-on.** Twelve criticisms with no ranking. → Cap blind spots at 6 named items, ranked by impact.
- **The framework parade.** Listing five frameworks for every observation. → Cite the framework once when it materially explains the verdict, then move on.
- **The recommendation tax.** Three improvements only. Not "and here are five more for good measure".
- **Source soup.** Don't dump raw Slack/email content. Summarise, cite, link.

## Operational notes

- **Date math.** Today is provided in the system prompt (`currentDate`). Compute window as `today - 7 days` unless Sir specified otherwise. Always print the explicit window in the headline ("4 May → 11 May 2026") so Sir can sanity-check.
- **Working hours assumption.** ~40 hours/week unless evidence says otherwise. State the assumption when computing the time-allocation table.
- **Parallel pulls.** Calendar, Gmail, Slack, Notion, Claude sessions, GitHub all run as parallel tool calls in a single message. Don't serialise.
- **Report file naming.** `~/Documents/Claude/performance-reviews/{YYYY-MM-DD}.md` using today's ISO date. If a file with today's date already exists, suffix `-2`, `-3` etc. — don't overwrite.
- **Prior report read.** `ls -t ~/Documents/Claude/performance-reviews/*.md | head -1` then read it before drafting section 7.

## What this skill is NOT

- Not a productivity coach. No advice on tools, calendars, or time-blocking strategies.
- Not a therapist. Doesn't psychoanalyse. Reports observable patterns, not motivations.
- Not a daily check. Weekly cadence. The compounding signal only shows up at 7-day grain.
- Not a public artifact. Reports are local. Never paste a report into Slack/Notion/email without Sir's explicit instruction.
