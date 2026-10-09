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

  
### Sprint Review
- **When:** Friday — 04:00 PM
- **Duration:** Maximum 30 minutes
- **Who:** Team + Product Owner / Evaluador Técnico
- **Goal:** Show what was built, verify database schema tracking against PostgreSQL literal output (§10), and collect feedback.

### Sprint Retrospective
- **When:** Friday — 04:30 PM (after the review)
- **Duration:** Maximum 45 minutes
- **Format:** What went well / What to improve / Action commitments
- **Rule:** Each retro produces at least 1 improvement action focused on resolving technical debt or pending tasks (§13).

### Backlog Refinement
- **When:** Wednesday — mid-sprint — 02:00 PM
- **Duration:** Maximum 1 hour
- **Goal:** Detail, trace, and estimate user stories for the next sprint, aligning with tasks from `tasks.md`.
- **Exit criterion:** The user story meets the Definition of Ready.

---

## Estimation

### Scale

| Points | Meaning |
|--------|---------|
| 1 | Trivial — Done in hours (e.g., adding text-only domain definitions §1) |
| 2 | Small — Done in one day (e.g., documenting existing single indices §6.2) |
| 3 | Medium — Takes 2–3 days (e.g., mapping complex constraints or shadow properties §3) |
| 5 | Large — Takes almost a full sprint (e.g., resolving multi-table structural debt D-2 / §13) |
| 8 | Very large — Should be split into smaller technical tasks |
| 13 | Epic — MUST be split before entering the sprint cycle |

**Technique:** Planning Poker
**Tool:** GitHub Issues & Projects

  3. Is anything blocking me with the tracking or constraints? (§4)
- **Rule:** Technical discussions happen after the daily, not during it.
