# Domain Map — Bounded Contexts

> **Why this document exists:** The domain map is the central Domain-Driven Design (DDD) artifact.
> It defines the system's logical boundaries, aggregates, and strategic subdomain values.
> In a modular monolith layout, boundaries protect software logic inside code folders rather than split network microservices.

---

## 1. Domain overview

Simple Stock Flow manages the complete transactional cycle of catalog stock management and immutable point-of-sale logging (§1). The business domain ensures strict historical accountability by preventing future catalog updates, price re-alignments, or category adjustments from corrupting historical transactions already closed by internal operators (§1 · Nombre congelado / ADR-004).

---

## 2. Identified Bounded Contexts (System Modules)

In this modular architecture, the system operates within a single central Bounded Context named **`Sales Management`**, containing highly cohesive logical modules focused on specific aggregate roots (§2 / §12).

### Bounded Context / Unified Module: Sales Management

| Field | Value |
|-------|-------|
| **Name** | `SalesManagement` |
| **Responsibility** | Governs product availability, processes transactions, and maintains immutable commercial sales historical logs. |
| **Owning team** | Core Engineering Team |
| **Architecture Layout** | Modular Monolith (.NET Core Application Assembly) — §12 |
| **Database** | PostgreSQL 16.14 (Centralized `sales` schema — §3) |
| **Ubiquitous Language** | Product, Category, Sale, SaleItem, User, Role, Frozen Name, Stock. |

**Context-specific terms (Ubiquitous Language):**

| Term | Meaning in THIS context | Notes |
|------|------------------------|-------|
| **Product** | Catalog item holding a price, stock count, and an optional image key (§1). | Restricted from feature creep (DP-03). |
| **Category** | Static reference category sembrada via migrations without CRUD maintenance (D-10 — §2.1). | Exactly 5 fixed system entries exist (§9.1). |
| **Price** | Decimal monetario representation vigente inside the catalog (`product.price` — §1). | Strictly positive, verified under `Money` object (§2.2). |
| **Sale** | Finalized, unalterable commercial record tracking who, when, and what was transacted (§1 / §2.3). | Deletion or editing is impossible (§7.1). |
| **SaleItem** | Internal transaction detail row locked tightly to its parent sales context (§2.4). | Copies and freezes catalog attributes at checkout (§1). |
| **User** | Authenticated internal operator running system transactions (§1). | Restricted to `admin` or `seller` roles (§2.5). |

---

## 3. Context Map

Since the system is a highly optimized modular monolith, there are no remote HTTP/REST microservice calls or network integration bottlenecks. Communication occurs **in-memory** across aggregate boundaries using direct domain adapter calls or a simple Mediator bus pattern (§0 / §12).

┌────────────────────────────────────────────────────────┐
│  SalesManagement Context (sales schema)                │
│                                                        │
│  ┌─────────────────┐             ┌──────────────────┐  │
│  │ User (Identity) │────(U)─────▶│ Sale (Checkout)  │  │
│  └─────────────────┘             └──────────────────┘  │
│                                           │            │
│                                          (D)           │
│                                           ▼            │
│                                  ┌──────────────────┐  │
│                                  │ Product(Catalog) │  │
│                                  └──────────────────┘  │
└────────────────────────────────────────────────────────┘

### Relationships table

| Module A (Upstream) | Relationship | Module B (Downstream) | Communication channel | Contract |
|-----------|-------------|-----------|----------------------|---------|
| `User` (Identity Aggregate) | `U → D` (Upstream supplies Operator Identity) | `Sale` (Transaction Aggregate) | In-Memory Object Call | `User.role` validation checks (§2.5 / T-20) |
| `Sale` (Transaction Aggregate) | `U → D` (Downstream calls to deduct catalog stock) | `Product` (Catalog Aggregate) | Atomic Domain Call | `Product.Withdraw` business routine (§2.2 / §2.3) |
| External Blob API | `ACL` (Anti-Corruption Layer) | `Product` (Catalog Aggregate) | Port-Adapter Interface | `image_key` simple lookup string (D-08 / §1) |

---

## 4. Core Domain, Supporting, Generic Subdomains

Strategic subdomain classification for business resource allocation:

| Type | Description | Investment | Subdomain / Module | Justification |
|------|-------------|-----------|--------------------|---------------|
| **Core Domain** | Where the business competitive advantage lies. | **MAXIMUM** | `Sale Checkout (Sale / SaleItem)` | Enforces absolute data immutability and historical frozen states, protecting records from downstream audit distortion (ADR-004). |
| **Supporting Subdomain** | Necessary for the core but not differentiating. | **MEDIUM** | `Catalog Management (Product / Category)` | Manages stock availability thresholds (`stock >= 0`) and categorization dependencies (FK-1 / ck_product_stock_non_negative). |
| **Generic Subdomain** | Commodity. Off-the-shelf solution layout. | **MINIMUM** | `Identity (User / Roles)` | Standard internal user account management and cryptographic hash handling without complex custom permissions (D-09). |

---

## 5. Modeling decisions

### Key decisions and discarded alternatives

| Decision | Discarded alternative | Reason |
|----------|----------------------|--------|
| **Modular Monolith Setup** | Distributed Microservices Architecture | A microservices approach would require complex distributed sagas, remote event buses, and multi-database tracing, which contradicts the simple architecture specification (§3 / §12). |
| **No runtime Category CRUD** | Dynamic category creation modules | Categories are fixed reference definitions sembradas via initial schemas; removing execution code keeps the catalog core fast and lean (D-10 / §2.1). |
| **In-Memory Aggregate Cross-Calls** | Decoupled asynchronous event broker loops | Since database state adjustments (stock withdrawals and sales rows) must be transactional, executing them on the same DB connection guarantees zero partial data corruption (ADR-001 / ADR-002). |

---

## 6. How to update this map

1. Before attempting to expand the schema or include fields beyond the minimum attribute policy, obtain approval against document constraints (DP-03).
2. If new internal business reports are planned, evaluate whether they can be computed directly inside the engine via read projections without duplicating raw tables (D-06).
3. The aggregate structures and boundaries defined here must align perfectly with the physical schema definitions verified against the database engine (§10).
