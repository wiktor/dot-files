# Note template and style guide (v3 — short form)

The vault note is written **in the user's own note style**: a mental-model
paragraph, headlines, workstreams in `[link] – verdict` lines, and a critical
coach. The machine ledgers at the end carry everything else; the reading part
does not repeat them.

Length budget (URL text excluded): the **reading part — title through Meetings —
≤ ~7 KB and ≤ ~90 lines** on any day. Inside it: Mental model ≤ 90 words,
Headlines ≤ 5 lines, Coach ≤ 10 lines, each workstream ≤ 5 lines, Open loops ≤ 8
lines. The ledgers and Sources come on top (a link-dense day is 20–30 KB on
disk; that is fine). If the draft is longer, cut in this order: adjectives and
connective prose → duplicate links → the fourth bullet of a workstream → the
sixth workstream (merge it into "Other") → mechanical open-loop items (summarise
as a count plus the one that matters). Never cut a Coach line that carries a
streak, or a `[link] – verdict` that carries a decision.

## Style rules (derived from the Daily Notes doc and the Aug 2026 notes)

-   **The unit is `[link] – verdict`.**
    `<Colleague F> wrote a doc: [Core and Jetski Remote](…) – good doc for 1P overall`.
    `1:1 <Colleague P> (those 1:1s are getting useless)`. One line, one link, one
    verdict.
-   **Their verdict beats yours — on judgment, not on facts.** Quote the daily log
    verbatim where they judged ("very useful", "this is meh"). Add the agent's
    verdict only where they left none, in italics with a leading `– agent:`. The
    doc is partial: a stream it does not mention is still written up from the
    systems of record.
-   **Critical first.** They read negative feedback well: the Coach opens with
    what was missed, named by moment and source, before anything that went
    right. No softening ("might consider"), no praise padding. Praise is allowed
    when earned and is as specific as the criticism.
-   **Numbers over adjectives.** "31 hand LGTMs, 22 on watcher-drafted CLs", "0
    own lines, 3rd day", "46 waiting, oldest 129 d".
-   **Plain sentences.** Short, active, one idea each. No "Crucially", no
    "Notably", no sentence that starts with "You". No bold labels inside bullets
    except the Coach's slot words.
-   **People** by first name or LDAP, once; collaborator lists live only in the
    Ledger. No LDAP soup in the reading part.
-   **Observable, not adjective** about others' work: the CL, the thread, the
    number — never the trait. Signals about people live in the machine block;
    the reading part names a person only inside a workstream fact.
-   Gemini notes label decision groups *Aligned* / *Needs Further Discussion* —
    the note writes **agreed**; the banned word never appears.

## Layout

```markdown
# YYYY-MM-DD — <Ddd> · <theme in ≤ 8 words>

> **Mental model:** <3–5 sentences, ≤ 90 words. What kind of day it was, the
> throughline, the one frame that changed, and the one thing that did not move.
> Written as they would write it to themselves — no hedging.>

## Headlines
1. **<what landed or was decided>** — <one clause of why it matters> — [link](…)
2. …  (3–5 lines; a headline is a decision, a landing, or a risk accepted — not a meeting)

## Coach
- **Missed:** <the unowned L2/L3 need, the silent thread, the risk stamped through — with the moment and the 10-minute fix> — [source](…)
- **Missed:** … (1–3 lines; the hard ones first)
- **Yesterday's move:** landed | slipped | silent — <what happened instead> — [source](…)
- **Focus:** <n>/5 areas (<area ~h · …>) · idle: <area> <n>th note · outside ~<h>
- **Brake:** set | not set — <the moment, one clause> — [source](…)
- **Leaked:** <the largest L0/L1 block, hours, who could have done it> (said <date>?)
- **Streak to break:** <pattern, count, dates> → <one structural fix: a rule, a person, or a slot>
- **This week:** <one area or stream — the artifact or decision — because <gap>>
- **Tomorrow:** <exactly one move — a decision or a conversation>
(≤ 10 lines; every line cites a streak ≥ 2 or a gap with a source — coaching_frame.md §5; ask-ups go to Open loops)

## Workstreams
### <Stream> — <one-line status: what moved · my part · what is open>
- [link](…) – <verdict or decision, verbatim when short>
- … (2–4 lines; one `cl/… – what they caught` line per substantive review comment)
### Other
- <single-line items that fit no stream: 1:1 outcomes, a stamp, a FYI> (≤ 4 lines)

## Open loops
- **On me:** <n> mentions unanswered — <the 1–3 that matter, one clause each with link>; <n> review requests without action (all watcher-drafted | list the exceptions)
- **Ask-ups:** <one per `leadership-ask: yes`, paste-ready: Diagnose — Propose — Ask (one short sentence each)>
- **My asks:** <what, of whom, no reply yet>
- **Promised:** <date-bound items, TODOs opened or carried, own CLs pending > 2 days>
(≤ 8 lines; every `replied: no` mention and every no-action review request is covered by a count or a link — the checklist still enforces it)

## Meetings
| time | meeting | with (roles for ≤ 6) | decision / next step | notes |
|---|---|---|---|---|
| 10:00 | … | … | … | [Full notes](…) ✓ / ✗ no notes / declined |

## Ledgers

<!-- leadership-ledger-start -->
### Focus touched today
- <stream> — <what/who/why> — status: on-track | at-risk
### Shipped / milestones (feeds "What's new")
- <landed> — focus: <stream> — <link>
### Advancing / next (feeds "What's next")
- <in-flight, ~month horizon> — focus: <stream>
### Blockers / risks / asks-up (feeds "Blockers, bottlenecks, external risks")
- <blocker> — needs: <decision/person/resource> — leadership-ask: yes | no
### Leadership evidence (feeds promo/perf packet)
- [tag] <what> — <why it mattered / scope> — <link>   (3–8 lines; the nine tags of leverage_model.md §6; a line without a link is not evidence)
### Collaborators today
- ldap1, ldap2, …
<!-- leadership-ledger-end -->

<!-- signals-start -->
- [kind] <me | ldap | project:stream | area:focus-area> — <observable> — <source> — <new | nth (dates)>
<!-- signals-end -->

### Sources & coverage
<one line per source with counts, from COVERAGE.md; meetings with ✗; docs read / skipped; date handling; bot-vs-own review split>
```

What moved where, relative to the v2 layout (so the rollups still find
everything):

v2 section                              | v3 home
--------------------------------------- | -------
Highlight one-breath                    | Mental model paragraph
What I did (TL view)                    | Headlines + the workstream status lines
Leaked time, Where I should have been   | Coach → *Missed* / *Leaked*
Coach's read                            | Coach (same five measurements, plus Missed, Leaked, Tomorrow)
Mode / Span / Mix                       | Sources & coverage (one line); the hours feed the Coach *Focus* line
Decisions & positions                   | Workstreams
Open loops (On me / My asks / Promised) | Open loops, compressed to counts + the items that matter
Ask-up one-liners                       | Open loops → *Ask-ups*
Leadership evidence (human section)     | Ledger only (`### Leadership evidence`)
Signals & patterns (human section)      | Signals machine block only; streaks surface in the Coach
What the room did without me            | one `– without me:` bullet inside the workstream it moved, when the room passed 300 msgs

## Worked fragment (Oct 23 2026, from real data)

```markdown
# 2026-10-23 — Fri · OneHarness comms turned outward, a hold stamped through

> **Mental model:** A calm home day that I spent talking *about* OneHarness instead of staffing it. Roles went to <Colleague E> in a DM, <Colleague F> got a FYI, <Colleague D> got a "moratorium" — three outward moves, zero recorded decisions. Meanwhile the launch moved to Nov 3 without me in the room, and I stamped <Colleague C>'s interim fix over the watcher's Blocking hold eleven minutes after it was placed. The desk ran late again: 21 review rows after 19:30.

## Headlines
1. **OneHarness roles proposed** — <Colleague B> rebases, I move clients off cortex steps; <Colleague E>: "This tracks heavily with me" — [DM](…)
2. **3P schedule tool: interim fix in, by hand** — cl/<id> stamped over a Blocking hold; <Colleague C> reverted their follow-up at 16:40 — [thread](…)
3. **Launch date Oct 29 → Nov 3** — decided at a standup I joined late; no gate list — [thread](…)

## Coach
- **Missed:** the AT-ST group — <Colleague J> signed the launch cal at 13:06, PWG comments left to <Colleague K>, 0 lines from me for the 3rd day — [group](…). Fix: the deletion-contract line + the JSP2/JSR owner, one message.
- **Missed:** <Colleague D> asked for a name to triage product-team harness CLs; a moratorium was stated, nobody named — [DM](…). Fix: one name in the reply.
- **Yesterday's move:** silent — the same AT-ST message, 3rd day — 4 of 5 moves slipped or silent (Oct 19, 20, 22, 23)
- **Focus:** 3/5 (roadmap ~1.25 h · graduation ~1 h · boundaries ~0.75 h) · idle: deployment 13th note · outside ~1.25 h (memory-team 1:1s)
- **Brake:** not set — the hold stamped through at 10:41; the date moved with no gates — 5th note not set
- **Leaked:** 22 of 31 hand LGTMs on watcher-drafted CLs; ~50 meme messages 12:47–14:50 — trusted-author list (said Oct 20, 21, 22)
- **Streak to break:** hand LGTMs on drafted CLs, 4th note → <colleague-g>, <colleague-h>, <colleague-i> join the trusted list; one 15:00 hand pass, nothing after 19:00
- **This week:** graduation — the Nov 3 gate list with owners in the AT-ST group — because that message slipped 3 days and Sonar is on day 15
- **Tomorrow (Monday):** post the gate list before the 09:00 standup — nothing else in that message
```
