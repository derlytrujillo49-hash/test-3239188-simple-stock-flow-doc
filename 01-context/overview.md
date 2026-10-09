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

