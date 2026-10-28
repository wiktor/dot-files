# Signals model — what the day says about my work, others' work, the system

A **signal** is one day's observable with a source. A **pattern** is the same
signal repeating across notes (or a count trending). The daily note can only
*record* signals and *count* repeats against the lookback; the weekly, monthly
and perf rollups do the rest — which is why every signal line is a sourced atom
in a machine block, not prose.

## 1. Rules

-   **Observable, not adjective.** "cl/<id> waited 3 days for its first
    review" — never "X is slow". "<Colleague B> drove the Oct 22 cut-line decision
    ([notes](…))" — never "<Colleague B> is great". The observable is reusable in perf
    feedback; the adjective is not, and is unfair without it.
-   **Sourced or absent.** Source = a URL (chat thread, CL, bug, doc, notes doc)
    when one exists; otherwise a reproducible pointer: `critique:<query>`,
    `cal:<date> <title>`, `mail:<date> <subject>`, `conv:<id>`,
    `doc:<fileId>#<section>`. Collector files in `/tmp` are not sources.
-   **Counts carry their query.** "66 CLs moved in my reviewer queue" is sourced
    as `critique:reviewer:me from:2026-10-06 to:2026-10-08`; "30 support-room
    messages by <colleague-a>" as `chat:<spaceId> 2026-10-07 senders`.
-   **Their verdict beats the agent's — when they left one.** When the daily log or
    a notes doc holds their judgment of someone's work ("good doc for 1P overall",
    "this is meh"), that *is* the `quality` signal — quote it, link the doc. The
    Daily Notes doc is partial by their own account: its silence is neither a
    verdict nor evidence that nothing happened. Facts come from the systems of
    record (chat, Critique, Buganizer, docs); the doc only ranks them.
-   **Streaks come from the lookback.** `lookback.md` carries the previous ten
    notes' signal blocks. Same subject + kind on an earlier day → `2nd (Oct 6)`,
    `3rd (Oct 6, 7)`. A streak ≥ 2 is a pattern and goes first in the
    human-facing section. Missing notes are gaps, not zeros: count by the notes
    present and name the missing days (`3rd (Oct 2, 6; no note Oct 3)`); a
    streak that matters can be re-checked against the system of record
    (`critique:`, `issues:`) rather than the notes.
-   **Three to twelve signals a day.** Zero about others' work is a finding (the
    day was spent alone); more than twelve means the bar is too low.

## 2. Vocabulary (20 kinds, four subjects)

Subject is `me`, an LDAP, `project:<stream>`, or `area:<focus area>`.

About **people's work** (subject = LDAP):

Kind       | Means                                                               | Typical source
---------- | ------------------------------------------------------------------- | --------------
`carrying` | did the heavy lifting: landed the thing, answered the room, owns it | authored CLs, `senders:` counts, notes Next steps done
`rising`   | took ownership beyond role, drove a decision, wrote the doc first   | thread link, notes Decisions, drive.md
`blocked`  | waiting on a decision, review, access, or person — with since-when  | thread, CL age, bug
`slipping` | stated date passed, "will do" repeated, bug aging without movement  | lookback + today's source
`asking`   | an ask placed (on me / on someone) and its reply latency            | thread link; `no reply 2d`
`quality`  | a verdict on an artifact — their first, others' second                | daily log line, doc comments, CL comments

About **projects and the system** (subject = `project:<stream>`):

Kind       | Means                                                                  | Typical source
---------- | ---------------------------------------------------------------------- | --------------
`momentum` | landed / decided / unblocked today                                     | CL, notes Decisions (agreed)
`stuck`    | decision pending across ≥ 2 meetings; *Needs Further Discussion* again | notes docs across days
`churn`    | the same debate reopened; a thread resurfacing                         | thread ids across days
`drift`    | scope or direction moved without a recorded decision                   | thread + notes
`load`     | the numbers: support msgs by person, review queue, DM volume, meetings | `senders:`, `critique:` query, `counts:`

About **me** (subject = `me`):

Kind        | Means                                                                    | Typical source
----------- | ------------------------------------------------------------------------ | --------------
`repeat`    | the same intention stated again ("offload OH") without a landed artifact | daily log + lookback dates
`leak`      | an L0/L1 block that failed the <Leader B> Test                             | timeline cluster
`late`      | review/chat clusters after 20:00 local, weekend work                     | timeline per-hour table
`delegated` | handed off with a DRI — tracked until it lands or slips                  | thread / daily log line
`promised`  | a date-bound commitment I made ("decision Monday")                       | thread / notes Next steps

About **coaching** (defined in [coaching_frame.md §4](coaching_frame.md); they
feed the Coach's read and the rollup's allocation table):

Kind      | Subject       | Means                                                                                        | Typical source
--------- | ------------- | -------------------------------------------------------------------------------------------- | --------------
`move`    | `me`          | yesterday's *One move*: `landed` / `slipped` / `silent`, and what happened instead           | lookback + today's thread / notes / timeline
`focus`   | `area:<name>` | L3 + L2 hours on one of the five focus areas today, or `0 h`; `area:outside` for the rest    | timeline clusters, evidence links
`brake`   | `me`          | `set: yes` / `set: no` at a braking moment (risk accepted, date reconfirmed, P0 unowned)     | notes Decisions / Next steps, thread
`sponsor` | LDAP          | a growth move for someone: DRI with visibility, public credit, doc ownership, level-up named | thread, notes, doc

## 3. Where each signal is read from

Collector file                          | Look for
--------------------------------------- | --------
`chat.md` `senders:` lines              | who answers in Support / 1P (`load`, `carrying`); if it is me → `leak`
`chat.md` threads                       | asks with `@` or `?` aimed at me and the gap to my reply (`asking`); "blocked", "waiting on", caps-lock, "UGH" from others (`blocked`); the same `th:` id or topic as a previous day (`churn`); long position statements by others (`rising`)
`chat_mine.md` `counts:`                | DM volume per person — attention went where (`load`); DM-only agreements → *codify it*
`chat_mentions.md`                      | every `replied: no` is an `asking` on me with the gap; mentions in spaces outside the fixed list show where the work moved (`drift`)
`chat_threads.md`                       | threads ≥ 8 msgs without me: who carried them (`carrying`), what stalled (`blocked`, `stuck`), what landed (`momentum`) — the room's day
`asks_ledger.md` + `reviews_mine.md`    | request → first-action latency per CL (`load` on me, `blocked` on the author); "no action by EOD" rows
`critique_pending.md`                   | CLs waiting on me, oldest first (`blocked` <author>, since <days>); my own pending CLs (`promised` me — stalled artifact)
`issues.md` top block, `issues_team.md` | bugs assigned to me today (`asking`); hotlist rows created / fixed / per assignee (`momentum`, `carrying`, `load` with numbers)
`docs_moving.md`                        | a doc of mine edited or commented by others while I was elsewhere (`rising` <editor>, `asking` when the comment names me)
`vault_edits.md`, `dailylog_diff.md`    | my own artifacts written today; TODOs I closed (`promised` → landed) or opened (`promised` new)
`reviews_mine.md`, `critique.md`        | review actions/day and after-20:00 share (`late`, `load`); authors I review most (`load` on me); authored CLs by others that landed (`carrying`); CLs sitting unreviewed (`blocked`)
`issues.md`                             | bugs touched again without state change (`slipping`); reassignments (`delegated` outcome)
`docs/*.md` (Gemini notes)              | *Decisions — agreed* (`momentum`), *Needs Further Discussion* repeated (`stuck`), *Next steps* owners (`asking`, `delegated`), `[Unassigned]` (`drift`), who carried the discussion (`rising`)
`dailylog.md`                           | their verdicts (`quality`), delegations ("Asking <Colleague Q> to…" → `delegated`), caps-lock risks (`drift`/`stuck`), intentions (`repeat` against lookback)
`jetski.md`                             | what agents did for me (`/cr` runs, fixes) — L0 via agents counts as `leak` only if someone else should have run it
`drive.md`                              | artifacts by others that appeared (`rising`, `momentum`; modifier unknown — say "appeared")
`timeline.md`                           | clusters after 20:00 (`late`), first→last span, alternating kinds per hour (context switching — note it under `leak` of the largest block)
`lookback.md`                           | streak counts; `delegated` / `promised` items from earlier days — landed, slipped, or silent; the previous note's *One move* → today's `move` (landed / slipped / silent, with the thread or notes that show it)
`timeline.md` L3/L2 clusters + evidence | hours per focus area (`focus`, one line per area touched, one `0 h` line per area idle ≥ 2 days per the lookback; `area:outside` when L3/L2 hours fit no area)
`docs/*.md` Decisions, `dailylog.md`    | braking moments — "accepted as a risk", a date reconfirmed, an unowned P0 — and whether a criterion, gate or DRI was recorded (`brake`)
`chat.md`, `docs/*.md`, `drive.md`      | growth moves for a person: named DRI in a leadership room, credit given, doc ownership handed over (`sponsor`)

## 4. Line grammar (machine block)

```markdown
<!-- signals-start -->
- [carrying] <colleague-a> — answered 18 of 24 Support threads today — chat:<spaceId> 2026-10-07 senders — 2nd (Oct 6)
- [blocked] <colleague-b> — 3P productionization waits on Sonar DRI since Oct 6 — https://chat.google.com/room/<spaceId>/<thread> — 3rd (Oct 6, 7)
- [quality] <colleague-q> — "Subagents in JPv2 vs Marina" draft: "good framing, wrong default" (daily log) — https://docs.google.com/document/d/<id> — new
- [stuck] project:telemetry — GDM/Core split marked Needs Further Discussion again — doc:<fileId>#Decisions — 2nd (Oct 6)
- [load] project:reviews — 69 review actions, 21 of them 19:00–20:00 — critique:reviewer:me from:2026-10-07 to:2026-10-08 — new
- [repeat] me — "i need to start offloading" OH — https://chat.google.com/dm/<space>/<thread> — 3rd (Sep 29, Oct 6)
- [delegated] me — Sonar delta scoping → <colleague-l>, due Fri — https://chat.google.com/room/<space>/<thread> — new
- [move] me — Oct 19 move "name the OH-rebase group at the standup": slipped — standup notes carry no names — doc:<fileId>#Next steps — 2nd (Oct 19)
- [focus] area:deployment — 0 h; deployment-tiers g3doc cl/<id> unsent day 14 — critique:author:me is:pending — 6th (Oct 13, 14, 15, 16, 19)
- [brake] me — set: no — 30-day TTL "accepted as a risk", no criterion or date recorded — doc:<fileId>#Decisions — new
- [sponsor] <colleague-i> — named standup owner in the launch-sync notes with <Colleague A> on leave — doc:<fileId>#Next steps — new
<!-- signals-end -->
```

One line per signal; `[kind] subject — observable — source — streak`. Several
sources are separated by `;`. The streak is `new` or `<n>th (<dates>)`.

## 5. Human-facing section

v3 notes (Oct 2026) have **no** human-facing signals section: the reading part
is Mental model → Headlines → Coach → Workstreams → Open loops → Meetings. The
machine block under `## Ledgers` is the only home of signals. Streaks ≥ 2
surface in the Coach (the *Missed*, *Streak to break* and *This week* lines); a
person appears in the reading part only inside a workstream fact. These lines
remain the raw material for peer feedback and for `People/<Name>/` notes; offer
to append them there, never write there unasked.

## 6. What the rollups withdraw

Every rollup also audits the period against the systems of record (Critique,
Buganizer, Drive, Calendar; Chat and mail for a week or on request) and marks
what the notes missed as `(not in daily notes)` — the notes are the index, not
the truth.

Period               | Reads                                                                          | Produces
-------------------- | ------------------------------------------------------------------------------ | --------
week                 | the week's full daily notes                                                    | headline, decisions, shipped, open loops carried over, **patterns with streaks**, **people signals by person**, load trend table, **focus-allocation table** + `move` scoreboard + `brake` count ([coaching_frame.md §6](coaching_frame.md))
month                | each note's Mental model (v2: Highlight one-breath line) + both machine blocks | the leadership template + appendices: Leadership evidence by tag, People signals by person, Project signals by stream, My patterns, **Coach's read for the period**
quarter / perf cycle | the months' rollups + every note's machine blocks                              | the same appendices over the range, deduped; evidence lines sorted by tag then date — the promo/perf packet draft
