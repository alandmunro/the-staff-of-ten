---
name: gotcha
description: Pressure-tests a PRD, spec, or implementation plan for hidden assumptions, ambiguities, and things likely to break, BEFORE any code gets written. Produces a structured "Gotcha Report" scoring risks by severity and likelihood, with concrete mitigation questions. Use this whenever the user has drafted or pasted a PRD, requirements doc, feature spec, or implementation plan and wants it reviewed, sanity-checked, stress-tested, or validated — or whenever they say things like "what could go wrong with this", "poke holes in this", "find the gotchas", "review this PRD before I build it", or "am I missing anything". Also trigger proactively before starting a nontrivial build when a PRD/spec exists but hasn't been through this review yet — catching a bad assumption on paper is far cheaper than catching it in code.
---

# Gotcha: PRD Risk Review

## Why this exists

The expensive mistakes in software aren't typos — they're wrong assumptions baked
into a plan before a single line of code exists. By the time a bad assumption
shows up as a bug, it's often load-bearing: other decisions were built on top of
it, and unwinding it costs far more than an hour of hard questions would have
cost up front. This skill's job is to ask those hard questions on paper, so the
build phase has fewer surprises and requires less rework.

Treat this as an adversarial read, not a supportive one. The goal is not to
validate that the PRD is good — it's to find the specific ways it's wrong,
incomplete, or ambiguous. A report that finds nothing wrong is usually a report
that didn't look hard enough; if a section of the input is thin, say so instead
of inventing confidence.

## Process

1. **Ingest the input.** Read the PRD/spec/plan in full (file, pasted text, or a
   conversation's worth of stated requirements). If it's genuinely thin — a
   two-line feature request rather than a real PRD — say so up front and ask
   whether to proceed with what's there or help flesh it out first. Don't
   silently pad a thin input with invented detail.

2. **Restate the goal and surface assumptions.** Write one or two sentences
   confirming what you understand the PRD to be asking for. Then separately
   list what the PRD *assumes* but never states — about the users, the data,
   the environment, other systems, who owns what, timelines. These implicit
   assumptions are usually where the real risk hides, more than the explicit
   requirements.

3. **Work through each risk category** in `references/risk-categories.md`.
   For each category, decide if it applies to this PRD at all — not every
   category is relevant to every project, and forcing findings into an
   irrelevant category produces noise, not signal. Where it does apply, ask
   the category's guiding questions against the actual content of the PRD,
   not in the abstract.

4. **Score each real finding** using the rubric in
   `references/risk-categories.md` (severity × likelihood). Findings that
   are technically true but trivial (typos, wording nitpicks) don't belong
   in the report — this is a risk review, not a copyedit.

5. **Separate findings from open questions.** Some things you can reason
   about and assess directly (e.g. "this assumes single-region deployment,
   which conflicts with the stated EU users requirement"). Others are
   genuine unknowns that only the user or a stakeholder can answer (e.g.
   "what should happen if the payment webhook arrives twice?"). Don't guess
   at the second kind and present it as a finding — list it as an open
   question instead. Guessing quietly is exactly the failure mode this
   skill exists to prevent.

6. **Write the report** following `templates/report-template.md` exactly —
   the structure is what makes this scannable under time pressure. Fill in
   every section; if a section has nothing to report, say "No material
   findings" rather than omitting it, so the reader knows it was checked.

7. **Update the gotchas log.** After the report is delivered (and especially
   after the user points out something the review missed, or after a build
   later reveals a failure the PRD review should have caught), append a
   short entry to `gotchas.md`. This is the running list of failure modes
   this skill has learned to look for — read it as part of step 3 on every
   run, and treat it as a living document, not a static reference. This is
   the mechanism by which the skill gets sharper with use instead of
   repeating the same misses.

## Example

`examples/sample-input.md` and `examples/sample-output.md` show a short PRD
and the report it should produce — use it to calibrate tone, level of
specificity, and how short a report can legitimately be when the PRD is
small. Don't treat its length as a target for unrelated PRDs; a bigger,
messier PRD should produce a longer report, and a tighter one should stay
short.

## Output

Deliver the Gotcha Report as a markdown file (or inline if the user is working
in chat and hasn't indicated a file is wanted). Name file outputs
`gotcha-report-<short-slug>.md`. Keep the tone direct and specific — cite the
exact line or requirement a finding refers to rather than describing it
vaguely, so the user can act on the report without re-reading the whole PRD to
find what you mean.

Do not treat "12 pages" or any other length as a target. Length should track
how much genuine risk surface the PRD has — a tightly-scoped, well-written PRD
for a small feature should produce a short report, and padding it out with
filler to look thorough is worse than a short, sharp one.

## After the report

Offer to do one of two things next, and let the user pick:
- Turn the highest-severity findings into a short list of PRD edits or
  clarifying questions to send back to whoever owns the PRD.
- If the user wants to proceed to implementation anyway, note which
  open findings are being knowingly accepted as risk, so that's a deliberate
  choice rather than something that quietly falls through.
