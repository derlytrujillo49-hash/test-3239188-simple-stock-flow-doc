# 02 — Problem Domain

> **What is this?** The mental model of the business. It is not technology — it is understanding
> the problem the system solves before writing code. This section comes from Domain-Driven Design (DDD).

## Why this section exists

The most costly mistakes in software are not bugs — they are domain misunderstandings.
When developers do not deeply understand the business:
- They create incorrect abstractions that have to be rewritten down the line.
- Names in the code do not match the business's terms, causing permanent confusion.
- Aggregate boundaries are drawn incorrectly, leading to data concurrency failures (§2 / ADR-002).

This section captures domain knowledge **before** executing technical persistence details.

---

## Key concepts mapped to Simple Stock Flow

**Entity:** Domain object with a unique identity thread (e.g., `Product` or `Sale` identified by a structural UUID — §2).

**Value Object:** Object with no identity of its own, defined purely by its attributes (e.g., `Money` wrapping price rounding rules — §2.2).

**Aggregate:** Group of entities treated as a transaction unit. Only the aggregate root can be referenced from outside (e.g., `Sale` governing `SaleItem` details — §2.4).

**Domain Event:** A finalized, unalterable business fact stated in past tense that occurred in the domain core (e.g., `SaleRegistered` or `StockWithdrawn` — §1).

**Bounded Context:** Logical boundary where a particular ubiquitous language model applies, implemented here as a clean decoupled module within a modular monolith layout (§12).

---

## What is here and how it is structured

### `domain-map.md` ⭐
Map of the logical system modules and their strategic subdomain classifications.
* **Filled in:** The unified `SalesManagement` context, tracking relationships between catalog components (`Product`), identity parameters (`User`), and checkout engines (`Sale`). It handles data flows using in-memory communication pathways instead of distributed networks (§0 / §5 / §12).

### `entities-and-rules.md` ⭐
Catalog of tactical building blocks, aggregates, value objects, and business invariants.
* **Filled in:** The 5 core system aggregates and entities (`Category`, `Product`, `Sale`, `SaleItem`, and `User`) along with critical transactional rules, such as enforcing non-negative stock counts and freezing price snaphosts at checkout time (§1 / §2 / §4).

### `events.md` ⭐
List of business events triggered by core aggregate roots.
* **Filled in:** Explicit payload contracts for system-wide notifications (like `SaleRegistered`). It details why asynchronous retry loops or Dead Letter Queues are skipped in favor of clean relational atomic transactions (§3 / §7.1 / ADR-001).

---

## Correlations with other sections

| This section feeds... | Why |
|-----------------------|-----|
| `01-context/README.md` | Provides the ubiquitous language dictionary used to outline system boundaries and scope. |
| `05-architecture/README.md` | Domain aggregate boundaries dictate the decoupled layout of core C# domain layers and infrastructure ports (§12). |
| `04-requirements/` | Core domain business invariants translate directly into strict user story acceptance criteria. |

---

## Questions this folder answers

- What are the main business entities? (`Category`, `Product`, `Sale`, `SaleItem`, `User` — §2).
- What rules can NEVER be violated? (Stock counts cannot drop below zero `stock >= 0`, and transacted details must remain fully frozen — §2.2 / §2.4).
- What important events occur in the domain? (`SaleRegistered` and `StockWithdrawn` — §1).
- Where are the system boundaries? (Contained inside a centralized monolithic `sales` engine schema — §0).

