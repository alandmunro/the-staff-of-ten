# Gotcha Report: [PRD/feature name]

**Reviewed:** [date] · **Input:** [file name / "pasted PRD" / "conversation"]

## What this PRD is asking for

[1-2 sentence restatement, confirming shared understanding before critiquing it]

## Assumptions the PRD is making but doesn't state

- [Assumption] — [why it matters if wrong]
- ...

(If genuinely none found beyond what's explicit, say so — don't stretch to fill this section.)

## Findings by category

Only include categories with real findings. For each finding:

### [Category name]

**[Finding title]** — *Severity: [Low/Med/High] · Likelihood: [Low/Med/High]*
[What specifically in the PRD creates this risk, quoting or citing the
relevant requirement. What breaks, and under what condition.]
→ *Mitigation / question:* [what would resolve this — a spec decision,
an edge case to define, a design change]

[repeat per finding]

## Open questions for the PRD owner

Things this review can't resolve on its own — genuine unknowns that need a
decision, not things being disguised as questions to avoid stating an
opinion.

1. [Question] — [why it can't be answered by inference alone]
2. ...

## Summary

- **Total findings:** [n] ([n] High priority, [n] Medium, [n] Low)
- **Biggest risk:** [the single finding most worth fixing before build starts]
- **Recommendation:** [proceed as-is / proceed after addressing High findings / needs rework before implementation]
