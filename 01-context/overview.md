# System Overview

> **Instructions:** Replace this content with your project's description.
> This is the first page someone new reads. They must be able to understand the system in 5 minutes.
> Remove these instructions when the document is complete.

---

## What is Simple Stock Flow?

Simple Stock Flow is a lightweight, immutable transaction system designed for internal inventory logging and sales registration (§1 / §2.3 / §12). The system prioritizes data consistency and long-term stability of financial reports, preventing catalog modifications from corrupting historical transactions (§1 · Frozen Name / ADR-004).

## Problem it solves

**Before the system:** Business processes relied on manual tools or flexible databases where changing a product's price or category name would retroactively alter the entire sales history, destroying the accuracy of past financial audits and reports (§1 / ADR-004).

**With the system:** The sales process is fully automated under a strict transactional immutability rule. Once a sale is captured, the product name, category name, and unit price are permanently frozen in that transaction line, providing reliable and stable reports over time (§1 / D-06 / ADR-004).


## Main users

| Role | Description | What they do in the system |
|------|-------------|--------------------------|
| **Admin** | Internal operator with elevated management and system setup privileges (§1 / §2.5). | Provisions system settings from the environment, registers authorized sellers, and monitors technical debt logs (§9.2 / §11.1 / §13). |
| **Seller** | Front-line transaction operator executing daily trades (§1 / §2.5). | Searches the inventory catalog, checks stock availability, and records immutable sales records in real time (§2.3 / §6.1 · Q1). |

## Technology stack

| Layer | Technology | Justification |
|-------|-----------|---------------|
| Backend | C# (.NET) | Enforces clean business domain constraints, logic execution, and object mappings without external leaks (§0 / §2). |
| Database | PostgreSQL 16.14 | Central relational engine hosting the physical schema where data constraints are strictly validated (§3 / §10). |
| ORM / Adapter | Entity Framework Core | Persistence adapter that maps application plural sets to singular relational tables (§0). |
| Infrastructure | Docker Compose | Containers hosting the database server under the `simple-stock-flow-db-1` node configured strictly in UTC (§3 / §12). |


## Current status

- **Phase:** In development (Physical schema successfully mapped and verified against the PostgreSQL engine — §10).
- **Current version:** v1.0.0-SDD
- **Last release:** 2026-09-19 (§12)
- **Next milestone:** Implementation of pending structural engine tasks, such as accounting foreign keys and category mappings (T-11 / T-12 — §4 / §5).

## Project contacts

| Role | Name | Contact |
|------|------|---------|
| Tech Lead | SDD Software Architect | techlead@simplestockflow.local |
| Product Owner | System Owner | owner@simplestockflow.local |
| DevOps | Database Administrator | dba@simplestockflow.local (§10) |
