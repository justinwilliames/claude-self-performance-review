> **⚠️ Moved (2026-07-03):** now maintained in github.com/justinwilliames/claude-skills — this repo is archived.

# claude-self-performance-review

A Claude Code skill that runs a weekly self-performance review on your actual work — pulling evidence from Slack, Notion, Gmail, Google Calendar, Claude Code sessions, and GitHub, then benchmarking against published performance and communication frameworks.

Honest. Opinionated. Evidence-backed. No horoscope language, no participation trophies, no manufactured praise.

## What it does

Ask Claude to "review my week", "how did I do this week", or run `/self-performance-review` and the skill produces a structured weekly report covering:

1. **Headline verdict** — one paragraph, honest call on what kind of week it was
2. **What shipped** — the output ledger (Grove's test: did the system actually change?)
3. **Time allocation** — meetings vs deep work vs async comms, with meeting density
4. **Communication audit** — outbound volume, tone sample with real sentences cited, asks-vs-answers ratio
5. **Decisions made** — Type-1 vs Type-2 (Bezos) with analysis-quality and speed verdicts
6. **Blind spots** — unanswered threads, dropped commitments, repeated patterns, topic avoidance (the section worth running the skill for)
7. **Last week's targets** — graded ✅ / ⚠️ / ❌ once a prior report exists
8. **Three improvements for next week** — specific, measurable, leverage-bearing. Three. Not five.
9. **Earned praise** — only when earned. Blank otherwise.

Reports are saved to `~/Documents/Claude/performance-reviews/YYYY-MM-DD.md` so each week's run can compare against the prior week's improvement targets.

## The frameworks it anchors to

These are the yardsticks. The skill cites them by name in the report so the logic is auditable.

- **Grove — High Output Management.** Output = activity × leverage. Activity is busywork; output is what shipped that affects the system.
- **Drucker — The Effective Executive.** Effectiveness = doing the right things, not doing things right.
- **Bezos — Type-1 vs Type-2 decisions.** Irreversible decisions deserve care; reversible ones should be fast.
- **Async-first communication hygiene.** Decisions documented, response latency tracked, tone calibrated against samples.
- **Cal Newport — deep work.** Meeting density and the preservation of uninterrupted 2hr+ blocks.

## What it pulls from

| Source | Used for |
|---|---|
| Google Calendar | Meeting hours, density, recurring vs ad-hoc, transcript links |
| Gmail | Outbound threads, top recipients, unreplied threads |
| Slack | Messages sent, channel activity, unreplied @-mentions |
| Notion | Pages created/edited, meeting notes with action items |
| Claude Code sessions | Topic clusters, what shipped, recurring problems |
| GitHub | PRs merged, tags cut, releases shipped across your active repos |

All pulled in parallel. Missing sources are flagged in the report, never hallucinated.

## Install

Drop the skill into your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills/self-performance-review
curl -L https://raw.githubusercontent.com/justinwilliames/claude-self-performance-review/main/SKILL.md \
  -o ~/.claude/skills/self-performance-review/SKILL.md
```

Or clone and symlink (recommended if you want to track upstream changes):

```bash
git clone https://github.com/justinwilliames/claude-self-performance-review.git ~/code/claude-self-performance-review
mkdir -p ~/.claude/skills/self-performance-review
ln -s ~/code/claude-self-performance-review/SKILL.md ~/.claude/skills/self-performance-review/SKILL.md
```

Also create the reports directory:

```bash
mkdir -p ~/Documents/Claude/performance-reviews
```

The skill auto-loads on every Claude Code session.

## Use

Trigger phrases Claude will recognise:

- `/self-performance-review`
- "Weekly review"
- "How did I do this week"
- "Dipstick on my week"
- "What did I ship this week"
- "Where are my blind spots"
- "Review my week" / "retro on my week"

Default window is the **last 7 days**. Override with "last 2 weeks", "last month", "since Friday", etc.

## What you'll need

This skill assumes you have the relevant MCP connectors configured in Claude Code:

- **Google Calendar** MCP
- **Gmail** MCP
- **Slack** MCP
- **Notion** MCP
- **Claude Code session management** (`ccd_session_mgmt`) — built in to Claude Code

GitHub data is pulled via the `gh` CLI, which you'll need authenticated locally.

If a source isn't configured, the skill will flag it in the report rather than fail. You'll still get a useful review from whatever's connected.

## What it deliberately won't do

- **No fake praise.** If nothing earns praise that week, section 9 is left blank with a placeholder. Manufactured praise debases real praise.
- **No generic advice.** "Improve focus" / "be more proactive" / "communicate clearly" are banned. Every recommendation names a person, a thread, a repo, or a decision.
- **No psychoanalysis.** Reports observable patterns, not motivations.
- **No public artifacts.** Reports are local. The skill never posts them to Slack, Notion, or email without explicit instruction.
- **No more than three improvements per week.** Discipline over breadth.

## The failure modes it actively avoids

Codified in [SKILL.md](SKILL.md):

- **The horoscope** — vague, applies to anyone, no evidence
- **The participation trophy** — "great week!" with no shipped output to back it
- **The pile-on** — twelve unranked criticisms (blind spots capped at 6, ranked)
- **The framework parade** — citing five frameworks per observation
- **The recommendation tax** — three improvements only, no "and here are five more for good measure"
- **Source soup** — dumping raw Slack/email content instead of summarising

## Contributing

This is an opinionated skill. PRs that sharpen the frameworks, add useful data sources, or improve blind-spot detection are welcome. PRs that soften the tone, manufacture praise, or add hedging language will be politely declined.

## License

MIT. Use it, fork it, improve it.
