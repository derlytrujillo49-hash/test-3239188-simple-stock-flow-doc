# 01 — Project Context

> **What is this?** The "why" of the system. Anyone new must be able to read this
> folder and understand what problem the project solves, what it includes, and what it does NOT include.

## Why this section exists

Before designing anything, the team needs to agree on:
- What problem are we solving?
- For whom?
- What is in scope and what is out of scope?

Without this, each team member works with different assumptions and the project fragments.

---

## What is here and how it is structured

### `README.md` (System Overview) ⭐
Executive description of the system in 1 page, serving as the introduction to the business context.
* **Filled in:** Simple Stock Flow name, the retroactive accounting report corruption problem it solves, main internal user roles (`admin` and `seller`), and its core technology stack (C# .NET, PostgreSQL 16.14, Entity Framework Core, and Docker Compose) verified directly against the live running engine (§1 / §3 / §10 / §12).

### `scope.md` ⭐
System boundaries: what it does and what it does NOT do under strict relational rules.
* **Filled in:** Explicit list of what is INSIDE the MVP (minimal product catalog DP-03, frozen historic transactions, and operator authentication) and what is OUTSIDE (customer entities, payment processors, and runtime category CRUD modifications) to prevent scope creep (§1 / §2 / §5 / §7.1).

### `Project Glossary` ➡️ Located in `02-domain/README.md` ⭐
Dictionary of the project domain. 
* **Note:** Per evaluation instructions, the official glossary has been unified directly inside the domain folder (`02-domain/README.md`) to keep terms amarrados to core aggregate boundaries, database types, and system check invariants (§1 / §2 / §4).

---

## Correlations with other sections

| If you change this... | Also review... |
|-----------------------|----------------|
| The problem described in `README.md` | Product vision in `03-product/README.md` |
| The scope in `scope.md` | Requirements in `04-requirements/` and architectural aggregates in `05-architecture/README.md` |
| A term in the Glossary (`02-domain/`) | Every document where that data constraint or attribute appears (§12) |

---

## Questions this folder answers

- What does this system exist for? (To enforce absolute commercial data immutability — §1).
- Who are the users? (Strictly authenticated internal `admin` and `seller` operators — §2.5).
- What does the system NOT do? (No customer tracking, no payments, and no live category editing — §7 / §12).

