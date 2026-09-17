---
title: Fabric
tags:
  - fabric
  - dp700

markmap:
  colorFreezeLevel: 3
  initialExpandLevel: 2
  activeNode: { placement: 'center'}
---

# Known gotchas <br> for the exam

A collection of choice-between-two-similar-features situations that DP-700 is likely to ask about, where two Fabric features resemble each other but the correct answer hinges on a specific detail. Update this file whenever a practice question is missed due to a wrong distinction (see the Workflow section in `AGENTS.md` and `study-progress.md`).

## **Preview features** 
- if an MCP lookup marks a feature as "(preview)", it's unlikely to be asked about on the exam unless it's already in wide use (see `AGENTS.md`).

## **Lakehouse SQL analytics endpoint vs. Warehouse**
- both use T-SQL, both store Delta-format data in OneLake, same engine underneath.
- Difference: the Lakehouse endpoint is **read-only** (no INSERT/UPDATE/DELETE, limited DDL — views/TVFs only) and is created automatically. Warehouse supports full DML/DDL and multi-table transactions, and has to be created explicitly.
- If the question talks about "making write-level changes with T-SQL" → the answer is Warehouse, not the Lakehouse endpoint.

## **Shortcut vs. Mirroring**
- Shortcut = a reference to an existing open-format source, doesn't copy the data, doesn't work for proprietary formats.
- Mirroring = brings an entire external database into Fabric as a unit;
- if the source is in a proprietary format, mirroring is the **only** option.
- The two can be combined ("mirror once, consume everywhere" pattern):
  - a central workspace mirrors the source,
  - other workspaces shortcut to the mirrored data instead of mirroring the same source again.

## **When NOT to use a shortcut/mirroring but a pipeline/dataflow/eventstream instead**
- Shortcuts/mirroring only solve data *availability*, not transformation or orchestration.
  - when complex multi-source transformation logic is needed (joins, business rules),
  - custom scheduling/orchestration,
  - a destination outside OneLake,
  - or real-time event streaming.

## **Dataflow Gen2 vs. Pipeline vs. Notebook**
- Dataflow Gen2: low-code/no-code, Power Query-based, suited to citizen developers and light transformation.
- Pipeline: orchestration (scheduling, chaining, activities — not heavy transformation itself, often calls a notebook or Dataflow Gen2 as part of the chain).
- Notebook: Spark code (PySpark/SQL/Scala/R), suited to complex transformation, large data volumes, testable logic.

## **T-SQL vs. PySpark vs. KQL for transformation**
- T-SQL: Warehouse/relational data, set-based operations.
- PySpark: Lakehouse, large/varied data, notebook-based data engineering.
- KQL: time-series/log data in Eventhouse, not relational data.

## **Native tables vs. OneLake shortcuts in Real-Time Intelligence**
- Eventhouse's native tables are optimized for fast ingestion and querying with KQL.
- Shortcuts bring data into Eventhouse from elsewhere in OneLake without copying it, but per MS Learn, queries over a shortcut can be slower than querying data ingested directly into Eventhouse (network calls to storage, no indexes).
- **Query acceleration** for OneLake shortcuts caches the data so performance is comparable to data ingested directly into Eventhouse
  - — without a separate ingestion pipeline or a permanent duplicate copy.

## **Database mirroring vs. metadata mirroring vs. open mirroring**
- database mirroring continuously replicates the data into Delta format (e.g. Azure SQL DB).
- Metadata mirroring only syncs the catalog's metadata, and uses shortcuts to reach the actual data rather than copying it (e.g. Databricks Unity Catalog).
- Open mirroring is a public API-based way to write change data into any mirrored-database item, regardless of the source system.

## **Deployment pipeline vs. Git integration**
- both fall under "lifecycle management" but sit at different layers, and don't replace each other:
- Git integration versions the content of workspace items (source control),
- deployment pipeline moves content between workspaces (dev → test → prod) as a promotion step.

## **Security layers are not the same thing**
- The layers stack, they don't replace one another — the exam often asks "what's used for WHICH scenario."
- workspace roles (Admin/Member/Contributor/Viewer, coarse level)
- vs. item-level permissions (e.g. sharing a single lakehouse)
- vs. OneLake data access roles (fine-grained folder/file-level access to the data itself)
- vs. row/column/object-level security in T-SQL
- vs. dynamic data masking (shows a masked value, doesn't block access to the row).

## **Domain assignment does NOT affect access**
- Assigning a workspace to a domain is purely an organizational/governance concept (data mesh grouping, filtering in the OneLake catalog, delegated tenant settings).
- It has **no effect** on who can see or access the workspace/items — that's still governed entirely by workspace roles and item-level permissions.
- All tenant users can see all domain *names*, even ones they have no role in.

## **Starter pool vs. custom pool (Spark & Apache Airflow jobs)**
- Same pattern shows up twice: Spark environment pools and Apache Airflow job pools.
- **Starter pool** = default, starts instantly, fixed size, auto-shuts-down after inactivity (~20 min for Airflow) → good for dev/light use.
- **Custom pool** = admin-defined size/autoscale/extra nodes, always-on until manually paused → good for production workloads.
- If a question emphasizes "instant start" or "shuts down when idle" → starter pool; "production," "always-on," "needs specific sizing/autoscale" → custom pool.

## **"Higher/lower granularity" means the OPPOSITE of what most people would assume, in Microsoft's usage**
- Microsoft consistently uses (confirmed in both the Fabric Warehouse dimensional-modeling documentation and the Power BI aggregation documentation — two separate teams/sources): **"higher granularity" = COARSER, a more aggregated level** (e.g. quarter, fewer rows) — not more fine-grained.
- "Lower granularity" = **more fine-grained**, closer to the atomic/transaction level (e.g. day or individual row).
- Example quotes: *"An aggregate fact table represents a rollup... to a lower dimensionality and/or **higher granularity**."* and, in Power BI, *"The Sales Agg table is at a **higher granularity** than Sales, so instead of billions, it might contain **millions of rows**."*
- This is the **opposite** of how many general BI/data sources use the term (e.g. some Kimball literature), where "high granularity" ~ "fine-grained" ~ lots of detail/small units.
- Practical rule for the exam: if an MS source/question says "higher granularity" with no other context, default to interpreting it as the **coarser/more aggregated** level in the hierarchy, not the more detailed one.
