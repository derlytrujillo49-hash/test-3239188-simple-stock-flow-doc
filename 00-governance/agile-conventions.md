# Agile Team Conventions — Simple Stock Flow

> Defines how the team works through its development cycles. Agree on and sign off
> with the entire team before the first sprint. Update when the team decides to change something.

---

## Sprint structure

| Field | Value |
|-------|-------|
| Duration | 1 week (Fast technical sprint cycles) |
| Sprint start | Monday |
| Sprint end | Friday (Strict delivery cycle) |
| Current sprint | Sprint 1 — Technical documentation and SDD reverse engineering |
| Estimated capacity | 15 story points per sprint |

---

## Ceremonies

### Sprint Planning
- **When:** Monday — 08:00 AM
- **Duration:** Maximum 1 hour
- **Who:** Entire development team
- **Goal:** Select and commit to sprint user stories from `tasks.md`, break down into precise technical database tasks (§4 / §13).
- **Output artifact:** Sprint Backlog updated in GitHub Projects.

### Daily Stand-up
- **When:** Tuesday to Thursday — 08:30 AM
- **Duration:** Maximum 15 minutes
- **Format:**
  1. What did I do yesterday to map the PostgreSQL schema? (§3 / §10)
  2. What will I do today regarding domain invariants? (§2)
  3. Is anything blocking me with the tracking or constraints? (§4)
- **Rule:** Technical discussions happen after the daily, not during it.
