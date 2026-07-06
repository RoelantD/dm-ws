# Package Manager Agent — Specification (baseline)

> **Part 4 baseline.** Refined spec with functional requirements, memory
> requirements, and bounded personas.

## Functional requirements

- **FR-PM-001 — Package lookup**: The system MUST return a package's current
  status, route, and condition given a package or order identifier.
  *Acceptance*: a known package returns exactly one record; an unknown
  identifier returns a clear "not found", never a guess.
- **FR-PM-002 — Order status**: The system MUST report an order's status
  (pending, in transit, delivered, exception).
  *Acceptance*: status matches the source record; ambiguous orders return a
  disambiguation prompt.
- **FR-PM-003 — Route awareness**: The system MUST identify which route a
  package is on and list other packages on that route when asked.
  *Acceptance*: only packages truly on the route are returned; none are
  invented.
- **FR-PM-004 — Label interpretation**: The system MUST parse a shipping label
  into structured fields (order, fragile flag, route).
  *Acceptance*: a valid label yields all three fields; a malformed label is
  rejected with a reason, not silently accepted.

## Memory requirements

- **MR-001 — User preference memory**: The system MAY remember a user's
  display preferences across sessions. *For whom*: that user only.
  *For how long*: until the user changes or clears it.
- **MR-002 — Role-based behaviour**: The system MUST apply role-based response
  rules from the user's role, not from stored personal data.
- **MR-003 — Working memory**: Within a conversation, the system MUST remember
  the order or route currently under discussion.

## Never store

- Real customer or employee PII.
- Credentials, tokens, or secrets.
- Anything a persona would use to hide safety- or compliance-critical information.

## Show for confirmation

- Before persisting a new long-term preference, the system MUST show the user
  what it will remember and ask for confirmation.

## Personas

| Persona | Preference | Behaviour change | Hard boundary |
|---------|------------|-----------------|---------------|
| **Stanley** | Minimise visual noise | Collapse statuses marked low-interest | MUST NOT hide safety-critical statuses |
| **Dwight** | Priority handling | Apply escalation policy on flagged orders | MUST NOT bypass approval rules |
| **Kevin** | Route trivia | Add "points of interest" annotation to route summaries | Informational only; MUST NOT change routing |
| **Warehouse staff** | Operational clarity | Concise, action-first language; no jokes | MUST NOT omit required warnings |

## Acceptance criteria

- **Correct use**: Remembered preference changes only presentation; user can inspect and clear it; nothing on the "never store" list is persisted.
- **Incorrect use (must fail review)**: Storing PII; persisting a preference without confirmation; letting a persona hide a safety-critical status.
