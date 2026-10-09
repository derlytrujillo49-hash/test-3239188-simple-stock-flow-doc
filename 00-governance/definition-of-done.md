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

