# Coaching frame — the target state the Coach block measures against

The rest of the skill records the **observed** state of a day. Coaching needs a
**target** state too: *target − observed → next step with a horizon*. This file
holds the target. It is an input, not prose to quote. The Coach block after the
Headlines (see [note_template.md](note_template.md)) is the only place the note
gives advice, and every advice line must cite a streak or a gap from here.

## 1. The five focus areas (target allocation)

Where the UTL's hours should go, in the user's own words from the Jul 2026
ecosystem-review prep. Each area maps to the streams the note already groups by;
the mapping is what makes allocation measurable.

Area         | What it is                                                                         | Typical streams / artifacts
------------ | ---------------------------------------------------------------------------------- | ---------------------------
`roadmap`    | roadmap and prioritization; what is in scope for a date and what is not            | go/jetski-prod-roadmap, milestone docs, cut-line decisions, "not in scope for Oct 22"
`deployment` | the 1P deployment framework — the "missing middle" between fishfood and GA         | deployment tiers g3doc, CodeMender / Marina / NotebookLM hosting decisions, ACE vs Capsule
`boundaries` | cross-team ownership: Agency ↔ JP ↔ Core ↔ Cloud, SDK ↔ Service layering           | ownership maps, OWNERS changes, "who owns X" decisions, lane rules
`scaling`    | the self-service model (Tier 0–3): docs, triage rotation, OH roster, support model | onboarding docs, process designs, delegations with a DRI
`graduation` | the production graduation path: what Fishfood → Teamfood → GA means and gates      | launch criteria, risk tabs, go/no-go decisions, PDD / Sonar / privacy gates

Rules:

-   **Count hours per area, not mentions.** Take the L3 and L2 blocks of the
    time mix and assign each to one area, or to `outside`. `outside` is a
    finding: either the list is stale or the focus drifted. Say which it looks
    like; do not decide for them. The list changes only when they say so.
-   **An area is "touched" at ≥ 0.5 h or one landed artifact with a link.**
    Zero-touch streaks are counted per area across the lookback (`focus` signal,
    §4).
-   **A stalled artifact belongs to its area.** A pending CL or unsent doc that
    serves an area (the deployment-tiers g3doc → `deployment`) is reported as
    that area idle for *n* days, not only as a stalled CL under *Promised*.

## 2. The second pattern: brakes

Longitudinal finding (Sep 2026, from the 1:1 reflections): strong at the
managerial acts that resemble thinking — strategy, framing, brokering — and
avoidant of the acts that slow something down: a launch or change gate, a
containment, "not yet". The daily note looks for the **braking moment** and
records what happened:

Braking moment available                            | Brake set (observable)                                       | Brake not set (observable)
--------------------------------------------------- | ------------------------------------------------------------ | --------------------------
a risk accepted in a meeting ("accepted as a risk") | a criterion and a date recorded ("30 d until Nov 3, then X") | the risk accepted with no criterion
a launch date reconfirmed                           | a go/no-go gate with an owner                                | the date restated, gates unnamed
a fast-moving engineer or CL stack                  | scope held ("first-class fields before this lands")          | reviewed and stamped as-is
"we don't understand the codebase" / caps-lock risk | a containment decision (freeze, pair, DRI)                   | "I see." and on to the next item
an unowned P0 named in notes                        | a DRI named in the thread the same day                       | carried to the next day's notes

Source for both columns: Gemini notes *Decisions* and *Next steps*, the daily
log, the thread where the decision would have been written. A `brake` signal
with `set: no` is not blame; it is the coaching input the user asked for.

## 3. Sponsorship check

Secondary finding from the same reflections: people who carry a stream for days
without a visible growth move (named as DRI with visibility, credited in a
leadership room, put on a doc as owner, a level-up named). The daily note
records the growth moves that happened (`sponsor` signal). The rollup joins them
against `carrying`: a person with `carrying` on ≥ 3 days and zero `sponsor` in
the period is listed under the Coach as under-sponsored, with the days.
Observable, not adjective — the same rule as every other signal.

## 4. The four coaching signals

Added to the vocabulary in [signals.md](signals.md). Same line grammar, same
machine block, so the rollups can withdraw them.

Kind      | Subject       | Observable                                                                               | Typical source
--------- | ------------- | ---------------------------------------------------------------------------------------- | --------------
`move`    | `me`          | yesterday's *One move* — `landed` / `slipped` / `silent` — with what happened instead    | lookback *One move* + today's thread / notes / timeline
`focus`   | `area:<name>` | hours today (L3 + L2) and the artifact, or `0 h`; `outside` for L3/L2 hours in no area   | timeline clusters, Leadership evidence links
`brake`   | `me`          | `set: yes                                                                                | no` — the braking moment and what was recorded
`sponsor` | LDAP          | the growth move: DRI named with visibility, public credit, doc ownership, level-up named | thread, notes, doc

Streaks for `move` count consecutive `slipped`; for `focus` consecutive `0 h`
per area; for `brake` consecutive `set: no`. Count them against the lookback
like every other signal; missing notes are gaps, not zeros.

## 5. Coach — the block and its rules

At most ten lines, under the heading **Coach**, right after the Headlines — the
third thing read every night. It opens with 1–3 *Missed* lines (the unowned
need, the silent thread, the risk stamped through: moment, source, 10-minute
fix), then the five measurements below, a *Leaked* line, and closes with *This
week* (one-week horizon) and *Tomorrow* (one move).

```markdown
## Coach
- **Missed:** <unowned need / silent thread / risk stamped through — moment, 10-minute fix> — <source>   (1–3 lines, first)
- **Yesterday's move:** <landed | slipped | silent> — <what happened instead, one clause> — <source>
- **Focus:** <n>/5 (<areas with hours>) · idle: <area> <n>th note (<dates>) · outside ~<h>
- **Brake:** <set | not set> — <the moment, one clause> — <source>
- **Leaked:** <largest L0/L1 block, hours> — <who could have done it> (said <date>?)
- **Streak to break:** <the longest pattern, with its count> → <the structural fix, one clause>
- **This week:** <one area or stream> — <the artifact or decision that would show it> — because <the gap or streak it closes>
- **Tomorrow:** <one move — a decision or a conversation>
```

Rules:

-   **Every line cites a streak ≥ 2 or a gap from §1–§3, with a source.** A
    coaching line without one is not written. "Focus this week" names the area
    with the longest idle streak or the open `leadership-ask: yes` with the most
    days — whichever has the larger number; say the number.
-   **One fix per streak, structural.** "Stop working late" is not a fix; "move
    the review window to 15:00–16:00 and let the bot hold the rest" is. The fix
    names a rule, a person, or a slot.
-   **The user's verdict wins.** When the daily log already names the week's
    focus, quote it and measure against it; do not replace it.
-   **No "You" sentences, no adjectives about them, no padding.** Same tone rules
    as [leverage_model.md §8](leverage_model.md). If a line would be empty
    ("Brake: no braking moment today"), write that in four words.
-   **Length**: the block ≤ ~1 KB (URL text excluded); one clause per slot.

## 6. What the rollups withdraw from this

Period | Produces
------ | --------
week   | a **focus-allocation table** (area × day, hours, `outside` column) · `move` scoreboard (landed / slipped / silent counts, the slipped ones named) · `brake` count set vs not set with the moments · people with `carrying` ≥ 3 days and 0 `sponsor`
month  | the same per week, plus **Appendix E — Coach for the period**: the three longest streaks with their fixes, the area with the fewest hours and what would have moved it, the under-sponsored people, and one *Focus next month* line measured against §1
