# Domain Events — Simple Stock Flow

> **Instructions:** Define here all business events triggered by the domain layer.
> In a modular monolith layout, domain events orchestrate in-memory side effects and state transitions
> across aggregate boundaries while maintaining strict data tracking rules.
> The name is ALWAYS in past tense and in the ubiquitous language of the domain.

---

## What is a domain event?

A **Domain Event** communicates that something important occurred in the business.
It is an immutable notification message that describes the fact in past tense.

✓ SaleRegistered
✓ StockWithdrawn
✓ UserRegistered
✗ RegisterSale (this is a command, not an event)
✗ ProductUpdated (too generic — what changed?)
✗ SaleEvent (does not indicate what occurred)

### Difference between Command and Event

| Concept | Intent | Tense | Can fail? |
|---------|--------|-------|-----------|
| **Command** | Instruction to trigger an operation | Present | Yes (e.g., if stock goes below zero §2.2) |
| **Event** | Notification of a finalized business fact | Past | No (it already occurred and is unalterable §1) |

Operator → [RegisterSale] → System → [SaleRegistered] → Local View Handlers
(Command)                    (Event)

---

## Event catalog

### Event: SaleRegistered

| Field | Value |
|-------|-------|
| **Name** | `SaleRegistered` |
| **Bounded Context** | `sales` (Core System Schema — §0) |
| **Aggregate** | `Sale` Root Aggregate (§2.3) |
| **Trigger** | Execution of `Sale.AddItem` followed by successful validation checks (§2.3). |
| **Consumers** | Local Sales Report projection components (§1 / D-06). |
| **Channel (topic)** | In-memory Mediator / Domain Events Dispatcher Bus |
| **Schema version** | `v1` |
| **Delivery guarantee** | Exactly-once (Guaranteed via the underlying DbContext relational transaction — §3 / ADR-001) |

**Payload (JSON schema):**

```json
{
  "eventId": "11111111-1111-4111-8111-111111111111",
  "eventType": "SaleRegistered",
  "aggregateId": "33333333-3333-4333-8333-333333333333",
  "aggregateType": "Sale",
  "occurredAt": "2026-09-19T21:53:44Z",
  "version": 1,
  "payload": {
    "soldBy": "string (operator account name §3)",
    "items": [
      {
        "productId": "uuid",
        "productName": "string (frozen value at sales point §1)",
        "categoryName": "string (frozen category name T-11)",
        "quantity": "integer (> 0)",
        "unitPrice": "numeric(18,2)"
      }
    ]
  },
  "metadata": {
    "correlationId": "uuid",
    "causationId": "uuid",
    "userId": "uuid"
  }
}
```

**Real payload example:**

```json
{
  "eventId": "88888888-8888-4888-8888-888888888888",
  "eventType": "SaleRegistered",
  "aggregateId": "99999999-9999-4999-8999-999999999999",
  "aggregateType": "Sale",
  "occurredAt": "2026-09-19T21:53:44Z",
  "version": 1,
  "payload": {
    "soldBy": "ana",
    "items": [
      {
        "productId": "22222222-2222-4222-8222-222222222222",
        "productName": "Hammer",
        "categoryName": "Herramientas",
        "quantity": 2,
        "unitPrice": 15.50
      }
    ]
  }
}
```

**What do consumers do with this event?**

| Consuming service | Action | Idempotent? |
|------------------|--------|-------------|
| **Sales Report Handler (D-06)** | Computes metrics dynamically by refreshing the read model view via query Q9 (§6.1). | Yes — Ensured natively by relational atomic transactions (§3). |

---

## Standard fields for all events

All events must include these fields in the envelope:

| Field | Type | Description |
|-------|------|-------------|
| `eventId` | UUID | Unique event identification string. |
| `eventType` | string | Event name in PascalCase (`SaleRegistered` / `StockWithdrawn`). |
| `aggregateId` | UUID | Primary key ID of the aggregate generating the fact (§3). |
| `aggregateType` | string | Core domain aggregate class type (`Sale` / `Product`). |
| `occurredAt` | ISO 8601 | Standard timeline entry synchronized strictly under UTC (§3 / §12). |
| `version` | integer | Incremental system version tracker for schema evolution. |
| `payload` | object | Concrete immutable transaction data (§1). |

---

## Event flow: Sales Confirmation Process

[Seller Operator]
│
│  RegisterSale (command)
▼
[Aggregate: Sale] ──(atomically calls)──▶ [Aggregate: Product]
│                                         │
│  SaleRegistered (event)                 │ StockWithdrawn (event)
▼                                         ▼
[Sales Report Handler]                    [Inventory Tracker]
Aggregates sales metrics (Q9)             Validates stock levels (ck_product_stock_non_negative)

---

## Schema evolution strategy

Events are system contracts. Changing them carelessly breaks internal components.

### What is a compatible change (does not break)?
* Adding a new optional metadata property to the structure.
* Supplementing the payload with a newly requested attribute like an image key (§2.2).

### What is an incompatible change (breaks)?
* Changing property values or altering decimal scales away from `numeric(18,2)` (§3).
* Removing core accounting fields such as quantity or unit prices (§1).

---

## Event summary table

| Event | Origin context | Topic / Medium | Consumers | Version |
|-------|---------------|----------------|-----------|---------|
| `SaleRegistered` | `sales` schema (§0) | In-Memory Mediator Bus | Report View Models (D-06) | v1 |
| `StockWithdrawn` | `sales` schema (§0) | In-Memory Mediator Bus | Product Catalog Hub (§2.2) | v1 |

---

## Policies — Reactions to events

A **Policy** describes what happens automatically when an event occurs ("whenever X occurs, do Y").

| Trigger event | Policy | Emitted command / Action | Service |
|--------------|--------|----------------|---------|
| `SaleRegistered` | Whenever a sale occurs, capture and freeze the category name. | Persist to `sale_item.category_name` (T-11 / ADR-004). | Domain Layer |
| `StockWithdrawn` | Whenever stock is modified, update the concurrency tracking token. | Mutate the row system state `xmin` (§3 / D-04). | Engine Layer |

---

## Resilience patterns for events

### Transactional Atomicity over Distributed Patterns
Because Simple Stock Flow is deployed as a modular single-instance application over a centralized PostgreSQL 16 database hub (§12), asynchronous network retry loops, Dead Letter Queues (DLQ), and message queues are omitted by design.

Instead, events are dispatched within the **exact same database transaction scope** managed by the Unit of Work pattern. If an event execution fails, the whole block (including product catalog adjustments and sales lines) undergoes a clean database rollback. This prevents partial data states or split historical entries without requiring complex distributed saga configurations (§2.3 / §3 / ADR-001).
