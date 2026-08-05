# Gotcha Report: Snooze on Notifications

**Reviewed:** example run · **Input:** examples/sample-input.md

## What this PRD is asking for

Add a "Remind me later" action to notifications that dismisses them and
re-shows the same notification after a fixed 1-hour delay.

## Assumptions the PRD is making but doesn't state

- Assumes notifications are re-deliverable as-is after a delay, rather than
  being tied to state that could go stale (e.g. a "your order shipped"
  notification is still accurate an hour later, but "you have an unread
  message" may not be).
- Assumes one snooze duration (1 hour) fits every notification type and every
  user, with no way to change it.
- Assumes the user will still be reachable/logged in when the snooze fires
  (app closed, logged out, notifications disabled in the meantime — none
  addressed).
- Assumes "all notifications" is a fixed, known set today, with no mention of
  whether new notification types added later automatically get this action.

## Findings by category

### Edge cases and error states

**What if the source event the notification represents is resolved before
the snooze fires?** — *Severity: Med · Likelihood: High*
E.g. user snoozes a "friend request" notification, then the request is
cancelled before the hour is up. The PRD doesn't say whether the snoozed
notification should still reappear, silently drop, or update. This is very
likely to happen at any real volume.
→ *Mitigation / question:* Define whether snoozed notifications are
re-validated against current state before re-delivery, or delivered as a
frozen snapshot.

**Double-snooze / repeated snooze** — *Severity: Low · Likelihood: Med*
PRD doesn't say what happens if the user snoozes the same re-appeared
notification again — does it stack, cap at N snoozes, or is it unlimited?
→ *Mitigation / question:* Decide a max snooze count or explicitly allow
unlimited.

### Concurrency and race conditions

**Snooze fires while the user is actively viewing the notification list** —
*Severity: Low · Likelihood: Med*
If the notification reappears while the list is open, does it need to
animate in, or can it silently exist and confuse a user who dismissed it an
hour ago and forgot?
→ *Mitigation / question:* Not necessarily a blocker, but worth a UX note so
support tickets ("this notification came back for no reason") aren't a
surprise.

### Backward compatibility and rollout

**"Ship in the next release" with no rollout plan** — *Severity: Med ·
Likelihood: Med*
No mention of a staged rollout, flag, or rollback path for a feature that
touches every notification and changes user-visible behavior for the whole
base at once.
→ *Mitigation / question:* Confirm whether this ships to 100% at once or
behind a flag, and what the rollback plan is if snooze scheduling misfires
at scale.

### Testing and observability

**Success metric isn't measurable as stated** — *Severity: High · Likelihood:
High*
"Users engage more with notifications" doesn't specify what's being
measured (open rate? tap rate? which notifications — snoozed ones only, or
all?) or what data currently exists to compute a baseline. As written, no
one can determine after launch whether this succeeded.
→ *Mitigation / question:* Define the exact metric, its data source, and the
baseline/target before build starts — this is the PRD's highest-priority gap
because it determines whether the feature can ever be judged to have
worked.

## Open questions for the PRD owner

1. Is 1 hour a fixed constant for every notification type, or should it vary
   (e.g. a payment failure vs. a social like)? The PRD states one number
   with no rationale — worth confirming it's intentional, not a placeholder.
2. What happens to a snoozed notification if the user's device is offline or
   the app is uninstalled when the hour elapses?

## Summary

- **Total findings:** 5 (1 High, 2 Medium, 2 Low)
- **Biggest risk:** The success metric can't currently be measured, so
  there's no way to know after shipping whether this worked — fix this
  before writing any code, not after.
- **Recommendation:** Proceed after addressing the High finding (metric
  definition) and getting an answer on stale-notification behavior; the
  rest can be resolved during implementation without blocking the start.
