# Risk categories and scoring rubric

Work through these in order. Skip categories that genuinely don't apply
rather than forcing a finding — but check each one deliberately before
skipping it; "doesn't apply" should be a conclusion, not a default.

## Scoring

Score every real finding on two axes, each Low / Medium / High:

- **Severity** — if this goes wrong, how bad is it? (data loss or corruption,
  security exposure, and unrecoverable user-facing breakage are High almost
  by definition; cosmetic or easily-reversible issues are Low.)
- **Likelihood** — how likely is this to actually happen given how the PRD
  is currently scoped, not how likely it is in the abstract.

Report severity × likelihood as the finding's priority (e.g. "High severity /
Medium likelihood"). Sort findings within each category by priority,
highest first.

## Categories

**1. Ambiguous or underspecified requirements**
Where does the PRD use language that two competent engineers could
reasonably implement two different ways? Vague verbs ("handle", "support",
"process"), unstated defaults, undefined terms specific to this domain.

**2. Hidden assumptions**
What does the PRD take for granted about users, data shape, scale, existing
systems, timing, or ownership — without ever stating it? These are the
riskiest because nobody consciously decided them; they just happened.

**3. Edge cases and error states**
What happens at the boundaries: empty input, first-time use, the very last
item, concurrent access, partial failure mid-operation? Does the PRD
describe the happy path only?

**4. Data model and schema risk**
Does the PRD imply a data shape that will need to change later? Are there
fields whose type, nullability, or uniqueness constraints aren't decided?
Migration risk for existing data if this touches something that already
has records in production.

**5. Integration and dependency risk**
What other systems, APIs, teams, or third parties does this PRD assume will
behave a certain way? What happens if they're slow, down, rate-limited, or
change their contract? Are there dependencies the PRD doesn't mention but
the feature clearly needs?

**6. Concurrency and race conditions**
Can two of the described actions happen at the same time in a way the PRD
doesn't address? Double-submits, simultaneous edits, out-of-order delivery.

**7. Performance and scale**
What does the PRD assume about volume — number of users, records, requests
per second — today and at plausible future scale? Does anything in the
design get quadratically worse, or hit an obvious bottleneck, as that
number grows?

**8. Security and privacy**
Who can access what, and does the PRD actually say so? New data being
collected or stored — is its sensitivity and retention addressed? Any new
trust boundary (new input surface, new auth path, new external caller)?

**9. Backward compatibility and rollout**
Does this change behavior existing users or systems depend on? Can it be
rolled out incrementally, or is it all-or-nothing? Is there a rollback
path if it needs to be reverted after release?

**10. UX and real-world usage gaps**
Beyond the primary user path, how does this behave for the user who
misunderstands it, does things out of the intended order, or has a
non-default setup (different locale, permissions, device, account state)?

**11. Testing and observability**
Once built, how would anyone know this is working correctly in production,
or notice quickly if it isn't? Does the PRD's success criteria translate
into something actually measurable?

**12. Cost and resourcing**
Does anything in the PRD imply a cost (compute, storage, third-party API,
engineering time) that isn't acknowledged, or that scales in a way that
could surprise whoever owns the budget?

**13. Timeline and scope realism**
Is the described scope actually achievable in whatever timeline is implied
or stated? Is there a smaller slice that delivers most of the value sooner,
that the PRD doesn't call out as an option?
