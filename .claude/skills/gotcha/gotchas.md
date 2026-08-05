# Gotchas log

A running list of failure modes this skill has learned to watch for, seeded
from common PRD-review misses and appended to over time — every time a
review misses something the user catches, or a later build reveals a risk
the PRD review should have flagged, add an entry here. Read this file as
part of step 3 in SKILL.md on every run.

Format: `- [date or "seed"] [what was missed] → [what to check for next time]`

- [seed] Reviews often accept a stated success metric at face value without
  checking it's actually measurable with data the system will have →
  always ask "where does the number for this metric actually come from."
- [seed] "Existing users" is often used in a PRD without defining what state
  those users' data is in, which quietly breaks migration assumptions →
  always ask what the current-prod data shape looks like before assuming a
  clean migration.
- [seed] PRDs for features that call a third-party API rarely specify
  behavior on rate-limit or outage → always check integration risk even
  when the PRD doesn't mention the dependency failing, since it usually
  won't.
- [seed] "Soft launch" or "small rollout first" is often stated without a
  defined rollback trigger or success bar to graduate past it → flag as an
  open question when a phased rollout is mentioned without exit criteria.
