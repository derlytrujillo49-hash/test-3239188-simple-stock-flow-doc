# System Scope — Simple Stock Flow

> **Why this document exists:** Scope prevents scope creep and aligns expectations.
> It is equally important to define what the system does NOT do as what it does.
> Review this document at the start of each planning cycle.

---

## In Scope

What the system **DOES build and maintain**:

### MVP Features

| # | Feature | Description | Responsible service |
|---|---------|-------------|---------------------|
| 1 | **Catalog Management** | Handles product inventory with a minimalist profile: name, price, stock, category, and an optional image key (§1 / DP-03). | `Product` Aggregate Core (§2.2) |
| 2 | **Immutable Sales Logging** | Records finalized transaction data by freezing names, categories, and unit values at checkout time (§1 / §2.4 / D-06). | `Sale` Aggregate Core (§2.3) |
| 3 | **Internal Identity Control** | Restricts resource operations to internal personnel using `admin` and `seller` closed roles (§1 / §2.5). | `User` Aggregate Core (§2.5) |

### Included integrations

| External system | Integration type | Purpose |
|----------------|-----------------|---------|
| **External Blob Storage** | Independent Binary Storage API | Hosts physical product images using opaque lookup strings (`image_key`) instead of saving raw byte arrays inside the engine database rows (§1 · Imagen / D-08 / §7.1). |

### Environments being built

| Environment | Purpose |
|-------------|---------|
| Local | Development on the developer's machine using local code layouts (§0). |
| Staging | Pre-production environment hosted on the `simple-stock-flow-db-1` Docker container running PostgreSQL 16.14 with the server clock locked in UTC (§3 / §12). |



## Out of Scope

What the system **does NOT build** in this version and why:

| # | What is out of scope | Reason | Future version? |
|---|---------------------|--------|----------------|
| 1 | **Customer Entity Profiles** | Sales are strictly linked to internal operators; no buyer data is stored (§1 / §7). | No — Outside core domain |
| 2 | **Payment Gateway Integration** | No external credit cards, bank protocols, or payment processors are included (§7). | No — Out of scope |
| 3 | **Category Management CRUD** | Category records are static seed lines loaded during initial migration without runtime maintenance (§2.1 / D-10 / §9.1). | Pending revision (§4.1) |
| 4 | **Sales Operator Breakdown** | Reporting models only aggregate totals per product to prevent exposing personal operator data (§7.1 / DP-02). | No — Blocked by privacy |

### What another system / team handles (and why not us)

| Feature | Who builds it | Why not us |
|---------|--------------|-----------|
| **Financial / BI Analytics** | External Analytics Team | Core domain aggregates and computes metrics directly in the database engine without data replication (D-06 / §7.1). |

---

## Scope assumptions

> These assumptions are taken to be true. If they change, the scope must be renegotiated.

| # | Assumption | Consequence if false |
|---|-----------|---------------------|
| 1 | Single-currency setup is permanent (D-05). | Secondary monetary columns and multi-currency value calculators would be required (§1 / §3). |
| 2 | All time logs utilize native UTC serialization (§3). | Regional offset converters and timezone mapping wrappers would change application logic (§12). |
| 3 | Initial catalog volume remains static at 5 seeded categories. | Category lookups and relational data mapping strategies might change (§9.1). |

