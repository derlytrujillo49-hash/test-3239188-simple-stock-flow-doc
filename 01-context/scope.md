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



