# Leverage model — how the daily note judges a TL day

The daily note is a **mirror, not a diary**. <User> already keeps the diary (the
Daily Notes doc). The agent's job is to show what cannot be seen from inside the
day: the shape of the hours, the repeats, the open loops, and the evidence. This
file is the rubric. It is deliberately short; apply it, don't quote it.

## 1. The Leverage Ladder (Grove)

A TL's output is the output of the people and teams they influence, not their
own keystrokes. Every block of the day gets a tier:

Tier   | Name           | What it looks like                                                                                                                                                                                                                               | Who else could do it
------ | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------
**L3** | Direction      | choosing between options (A/B/C → B), scope cuts, "we won't do X", roadmap/northstar, framing a problem for leadership (ask-up), ownership rules ("owners ≠ approver for Cortex")                                                                | only the UTL
**L2** | Multiplication | writing the doc that answers the question N times (milestones doc, PDD delta, onboarding doc, g3doc fix), designing a process (triage rotation, OH roster), delegating *with context* and a named DRI, coaching in a 1:1, unblocking by decision | mostly the UTL
**L1** | Coordination   | standups, syncs, office hours, status, routing a question to the right person, in-lane LGTMs on trusted authors, answering a question once                                                                                                       | many on the team
**L0** | Execution      | writing code, personally fixing an incident someone is already on, support answers in the room, out-of-lane reviews, manual ticket edits, re-deriving something a doc should hold                                                                | anyone

**How to assign hours.** Use `timeline.md`: meetings have durations; chat and
review actions are point events — cluster them (≥3 actions within 30 min = one
block, charged at the span). Round to half hours. State the mix as hours, not
percentages ("L3 ~1h · L2 ~1.5h · L1 ~3h · L0 ~1h"), and say what the *largest
L1/L0 block* was. Do not invent a target percentage; compare against the user's
own *"Focus for today"* / *"Goal for today"* line in the daily log when there is
one — the doc is not kept every day; if the line is missing, compare against
yesterday's *One move* from the lookback and say which was used.

## 2. The <Leader B> Test

> "Am I the only person who can do this?" — If no → it is delegable. If yes → do
> it, and name it as a choice.

<Leader B>'s own words (Jul 25 2026): client engagements and support are "not a
good use of [your time], because (a) the impact is limited to a single user, and
(b) many people on the team can play that role." Apply it per L0/L1 block.
**Leaked time** = L0/L1 blocks that fail the test. Name the person who could
have done it (from today's collaborators) and whether the user has already said
so before (check `lookback.md`).

Repeat-offender list (from the Jul 2026 evidence file; update when it changes):
support-room answers, CDPush/incident fixes someone else owns, running Office
Hours solo, out-of-lane CL stamps, reading 7 docs and producing 0 artifacts.

## 3. Missed (where I should have been)

The highest-leverage need that had **no owner** today. Sources: Gemini notes
`Next steps` tagged `[Unassigned]`/`[The group]`, risks the user wrote in the
daily log in caps ("OOOPS SONAR", "UGH fighting"), questions from senior
stakeholders with no reply (<Colleague D>'s "what's the ETA" went unanswered in Jul),
`leadership-ask: yes` items from the lookback that are still open. One or two
lines, each with the 10-minute version of what would have closed it.

## 4. Ask-up shape: Diagnose → Propose → Ask

Every escalation in the note is written as the one-liner the user could paste to
<Leader B> / <Leader C> / <Leader A>:

> *Diagnose* — Sonar sign-off is required for the Oct 22 3P launch and has no
> DRI. *Propose* — <colleague-l> owns the Sonar delta scoping this week; I review. *Ask*
> — who owns the Sonar relationship on the AGY side?

The **Ask is a decision, a person, or a resource** — never a task for the user.
Anti-patterns: "Will you send the 2-line note today?" (micro-task), five P0s (no
allocation), softening ("might consider").

## 5. Mode and language

IC mode leaks into language: "I solved", "looking", "green now", "CLs
submitted". Leader mode: "we", "what's your use case", "we don't have this
planned", "not in scope for Oct 22". Quote one line from `chat_mine.md` as the
day's mode signature. Fire-fighting days push strategic work to late night —
report the span (`timeline.md` first → last action) and whether review/chat
clusters sit after 20:00 local.

## 6. Leadership evidence — what counts, how to tag

Purpose: feed the monthly rollup and the promo packet with *linked* examples. A
line is evidence only if it has a link (thread, doc, CL, bug, notes doc).
Without a link it goes to **Patterns → "codify it"** — the Context Evaporation
Trap (agreements brokered in DMs that evaporate).

Vocabulary (keep it to these nine; they map to the three UTL shifts and to
senior-staff expectations of scope, direction, influence and people growth):

Tag          | Means                                                  | Example (Oct 5–7 2026)
------------ | ------------------------------------------------------ | ----------------------
`direction`  | chose/changed direction, scope cut, option picked      | Option B (AT-ST → Sawmill) after writing A/B/C; "not JPv3"
`ask-up`     | escalated with diagnose→propose→ask                    | "one JP engineer × 4 weeks" for the LS↔LH gap
`boundary`   | said no / declined / redirected with a rule            | 4 out-of-lane CLs reassigned to matthewli
`delegation` | handed off with context + DRI                          | `Subagents in JPv2 vs Marina` → <Colleague Q>; CLIs → <Colleague O>
`artifact`   | durable doc/framework/g3doc published                  | `Jetski Remote <> Core Milestones`; PDD deltas
`unblock`    | decision/approval that unblocked others                | OWNERS file so Core isn't blocked (cl/<id>)
`people`     | coaching, growth, onboarding                           | onboarding doc for <colleague-m>; <Colleague A> 1:1 reset
`influence`  | moved a decision in another org                        | Core/GDM telemetry split; Agency BYO-storage correction
`judgment`   | first-principles framing that changed the conversation | "~3M trajectories/day, ~35/s peak" estimate; platform-vs-product read on OneHarness

Format: `- [tag] what — why it mattered / scope — link`. One line. Three to
eight per day is normal; zero is a finding.

## 7. Patterns (longitudinal)

`lookback.md` carries the previous five notes' forward-looking blocks. Check:

-   **Repeated intentions**: the same "will hand off / will write / will decide"
    appearing ≥2 times without a landed artifact → name it with the dates.
-   **Trend counts**: support-room messages/day, review actions/day, work span,
    meetings/day. Report the number and the direction, not a judgment.
-   **Loops**: `leadership-ask: yes` items from previous notes that are still
    open; "decision on Monday" that did not happen.
-   **Streaks**: positive ones count too ("0 support-room messages for 3 days").

Patterns stay descriptive here. The one place the note turns them into advice is
the **Coach** block (§9).

## 8. Tone

Candid, specific, short. Name the trap, cite the timestamp, propose the
structural fix, ask one question. Praise is allowed when earned and must be as
specific as criticism. Never pad.

## 9. Coach — target minus observed

Sections 1–7 produce the observed state and the "what not to do" half of the
target (the <Leader B> Test). The "what to do" half — the five focus areas, the
brakes pattern, the sponsorship check — lives in
[coaching_frame.md](coaching_frame.md). The note's **Coach** block (≤ 10 lines,
after the Headlines) opens with what was *missed*, then scores yesterday's *One
move*, names focus areas touched and idle, the braking moment, the largest leak,
the streak to break with its structural fix, the focus for **this week** and one
move for tomorrow. Each line cites a streak ≥ 2 or an allocation gap, with a
source; otherwise it is not written. Coaching lines are withdrawable by the
rollups because they are also deposited as `move`, `focus`, `brake` and
`sponsor` signals in the machine block.
