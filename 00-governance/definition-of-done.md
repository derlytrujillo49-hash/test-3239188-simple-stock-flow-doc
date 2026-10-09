# Definition of Done (DoD) — Simple Stock Flow

> A User Story is **DONE** when it meets ALL criteria on this checklist.
> If even one is missing, the story is NOT done — it goes back to In Progress.

## Mandatory checklist

### Code
- [ ] Code implements all acceptance criteria of the user story, respecting the singular naming convention for tables (§0).
- [ ] Code was reviewed and approved by at least 1 team member through a Pull Request (PR) review.
- [ ] Code follows project standards, ensuring that data types match PostgreSQL definitions (e.g., `numeric(18,2)` for prices and `integer` for quantities §3).
- [ ] No technical debt or missing constraint is introduced without registering it as a declared debt in the data model (§13 / T-20).

### Tests
- [ ] Unit tests written for new business logic and core aggregate invariants (such as `Product.Withdraw` or `Sale.EnsureConfirmable` §2.2 / §2.3).
- [ ] Test coverage does not decrease from the project baseline.
- [ ] All tests pass locally and in the Continuous Integration (CI) environment.
- [ ] Acceptance criteria verified against the database schema rules, making sure the `ck_product_stock_non_negative` check never breaches (§2.2 / §4).


### Integration
- [ ] Changes do not break other domain aggregates or service boundaries (integration tests pass across core boundaries §2).
- [ ] If API changes: OpenAPI contract updated in `07-api/contracts/`.
- [ ] If data model changes: the centralized schema definition is validated against the PostgreSQL 16 engine requirements (§3 / §10).
- [ ] If new/modified events: `event-catalog.md` updated.

### Deployment
- [ ] Code is mergeable to `dev` with zero conflicts.
- [ ] CI/CD green on the branch, verifying that migrations apply flawlessly to the database structure (§3.2).
- [ ] Deployed to staging environment (`simple-stock-flow-db-1` container or equivalent infrastructure §12).
- [ ] Basic smoke test passing on staging, including verified seeding for category reference data (§9.1).

### Documentation
- [ ] Service `README.md` updated if the public interface changed.
- [ ] If a significant technical decision was made (e.g., changes to physical indexing or soft-delete policies): ADR created or updated inside `05-architecture/decisions/records/` (§0 / §2.2 / §6.2).

