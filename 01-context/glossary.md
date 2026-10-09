# Project Glossary — Simple Stock Flow

> **Instructions:** Define all technical and business terms used in the project here.
> This is the official dictionary—in the event of ambiguity, this document takes precedence.
> Add terms throughout the project, not just at the start.

---

## How to use this glossary

1. Before using a technical or business term in code, documentation, or conversations: look it up here.
2. If it is not listed: add it along with its definition.
3. If there is a disagreement regarding the definition: discuss it as a team and update this document.

---

## Domain terms

| Term | Definition | Notes / Synonyms |
|------|-----------|-----------------|
| **Product** | Catalog item with a name, price, stock level, category, and optional image (§1). | Do not include additional attributes such as SKU or description (DP-03). |
| **Category** | Fixed classification (one of five options) to which a product belongs; seeded at the database level and not subject to maintenance (§1 / D-10). | No operational CRUD functionality for creation or deletion (§2.1). |
| **Price** | Current monetary value of the product in the catalog. Must be strictly positive (§1). | Represented by the `Money` value object (§2.2). |
| **Stock** | Available units of a product in the catalog. Cannot be negative (§1 / §2.2). | Constraint enforced by the engine via `ck_product_stock_non_negative` (§4). |
| **Product Image** | Opaque key for an externally stored binary file; represented as `NULL` if absent (§1 / D-08). | Avoid storing relative paths or raw bytes (§1). |


| **Sale** | A completed, immutable commercial transaction recording who, when, and what was sold. Once recorded, it cannot be edited or deleted (§1 / §2.3). | Not to be confused with quotes or intermediate states. |
| **Sale Line** | A sale line item that stores a frozen copy of the product, quantity, and price from the moment of the transaction (§1 / §2.4). | Internal object; has no independent existence outside its sale (§2.4 / FK-2). |
| **Quantity** | Units sold in a sale line. Must be strictly positive (§1). | Encapsulated in the `Quantity` value object (§2.4). |
| **User** | Internal operator who authenticates on the platform and records commercial transactions (§1 / §2.5). | No client or end-buyer entity exists (§1). |
| **Role** | User attribute restricted to a closed set of two options: `admin` or `seller` (§1 / §2.5). | The `admin` role is provisioned by the deployment environment, not at runtime (§11.1). |

---

## Technical terms of the project

| Term | Definition |
|------|-----------|
| **Hexagonal Architecture** | Architectural style (Ports and Adapters) that isolates pure domain rules from infrastructure and database details (§2.5 / §12). |
| **Aggregate Root** | Main domain entity that defines consistency boundaries and controls access to its internal components (§2.2 / §2.3). |
| **Frozen Name** | Literal copy of product attributes at the time of sale to protect historical reporting against catalog changes (§1 / ADR-004). |
| **Logical Deletion** | Mechanism that marks a product as inactive via the `deleted_at` column without removing the physical row, preserving historical integrity (§2.2 / ADR-003). |
| **Optimistic Concurrency** | Pattern for controlling simultaneous modifications in the database engine using the `xmin` system column as a witness (§3 / D-04 / T-10). |
| **Shadow Property** | Field defined purely in the persistence mapping that is not exposed in the application domain entities (e.g., `deleted_at`, `xmin` — §2.2 / §3). |
| **Seed Data** | Static information required for initial system operation, loaded directly during database migration (§2.1 / §9.1). |
| **Single-Currency Value** | System constraint assuming a single operating currency by design across all financial tables and objects (§1 / D-05). |


---

## Acronyms

| Acronym | Meaning |
|---------|---------|
| **API** | Application Programming Interface |
| **CRUD** | Create, Read, Update, Delete |
| **SDD** | Software Design Description (§12) |
| **ADR** | Architecture Decision Record (§12) |
| **PR** | Pull Request |
| **DoD** | Definition of Done |
| **DoR** | Definition of Ready |
| **FK** | Foreign Key (§5) |
| **PK** | Primary Key (§4) |
| **UTC** | Coordinated Universal Time (Standard Server Time Zone — §3 / §12) |
| **ORM** | Object-Relational Mapping (§0) |
| **UUID** | Universally Unique Identifier (§3) |
| **D-xx** | Project-Specific Technical Decision (ej. T-20 — §4) |
