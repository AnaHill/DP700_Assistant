---
title: DP-700 Topics <br> by product area
tags:
  - fabric
  - dp700
markmap:
  colorFreezeLevel: 2
  initialExpandLevel: 2
  activeNode: { placement: 'center'}
---

Same items as in `exam-domains.md` (the official source), just reorganized by Fabric product area to support studying — update `exam-domains.md` first if MS Learn changes the skills-measured list, this file follows along after.

# OneLake
- MS Learn link [fabric/onelake](https://learn.microsoft.com/en-us/fabric/onelake/)
- One logical data lake, zero-copy for every engine
- Shortcut vs. Mirroring — decision criteria (see [known-gotchas.md](known-gotchas.md), [unify-data](https://learn.microsoft.com/en-us/fabric/onelake/unify-data "OneLake Unify data"))
- [Mirroring](#mirroring) section: database - metadata - open mirroring
- Metadata shortcut (e.g. Databricks Unity Catalog) vs. a regular shortcut (e.g. ADLS Gen2) — different source type, same shortcut mechanism
- Shortcut transformations (file-based, AI-powered)
- OneLake security / data access roles (RBAC: T-SQL endpoint, Spark, Power BI, File Explorer, Excel)

# Lakehouse <br> (Data Engineering)
- MS Learn link [fabric/data-engineering](https://learn.microsoft.com/en-us/fabric/data-engineering/)
- Structure: `/Files` (bronze) vs. `/Tables` (Delta, gold)
- Notebooks: PySpark `%%pyspark`, Spark SQL `%%sql`, Scala `%%spark`, R
- Delta Lake: ACID, schema enforcement, time travel
- SQL analytics endpoint (read-only) vs. Warehouse
- Table maintenance: `OPTIMIZE, VACUUM`, [V-Order and Z-order](https://learn.microsoft.com/en-us/fabric/data-engineering/delta-optimization-and-v-order?tabs=sparksql)
- [Spark Structured Streaming best practices](https://learn.microsoft.com/en-gb/fabric/data-engineering/structured-streaming-best-practices?tabs=python) — one of the two streaming routes, see [Decision guides](#decision-guides)
- Windowing functions, see [learn](https://learn.microsoft.com/en-us/stream-analytics-query/windowing-azure-stream-analytics):
  - **Tumbling**: fixed length, no overlap, an event belongs to exactly one window (note the `offset` parameter); this and the others (except snapshot) look like `GROUP BY Topic, TumblingWindow(second, 10)`
  - **Hopping**: fixed length + hop size, with overlap — if hop size = length, it behaves like tumbling
  - **Sliding**: output only when the window's contents actually change (an event enters/leaves), no fixed cadence
  - **Session**: groups events that arrive close together, with timeout + max-duration parameters — the max-duration check runs on the same interval as max duration itself → the actual duration can be up to 2x maxDuration
  - **Snapshot**: groups events that arrive with the same timestamp (`GROUP BY ..., System.Timestamp()`), has no dedicated function+parameter combo of its own!!!
- Denormalization, grouping/aggregation
- Duplicate / missing / late-arriving data
- Identifying and fixing notebook errors
- Spark
  - Spark performance optimization
  - Compute-optimized nodes are **not ideal for data-intensive Spark** jobs *compared to memory-optimized nodes* [link](https://learn.microsoft.com/en-us/training/modules/use-apache-spark-work-files-lakehouse/2-spark "Apache Spark")
  - > Enabling **native execution engine** in environment settings optimizes **performance for complex** Spark jobs **without** code modifications.

# Data Factory <br> (Orchestration & Ingestion)
- MS Learn link [fabric/data-factory](https://learn.microsoft.com/en-us/fabric/data-factory/)
- Pipelines: activities, the Copy activity, parameters, dynamic expressions
- Schedules vs. event-based triggers
- Dataflow Gen2 (Power Query, low-code)
- Choosing an orchestration tool: Dataflow Gen2 vs. pipeline vs. notebook, <br> see [Decision guides — cross-cutting choices](#decision-guides) — a different skill from choosing a transformation tool
  - > Dataflow Gen2, while useful for data preparation and transformation, does **not directly support creating new tables from combined data** sources.
- [Apache Airflow jobs](https://learn.microsoft.com/en-gb/fabric/data-factory/apache-airflow-jobs-concepts) — the successor to ADF's Workflow Orchestration Manager
- Designing and implementing full vs. incremental data loads
  - data warehouse watermark, see [dw watermark ingestion example](https://learn.microsoft.com/en-us/fabric/data-factory/tutorial-incremental-copy-data-warehouse-lakehouse)
- Identifying and optimizing pipeline errors

# Warehouse
- MS Learn link [fabric/data-warehouse](https://learn.microsoft.com/en-us/fabric/data-warehouse/)
- T-SQL: DDL/DML/DQL, ACID, multi-table transactions
- Dimensional modeling: fact/dimension tables, star schema
- [Row-level](https://learn.microsoft.com/en-gb/fabric/data-warehouse/row-level-security), [column-level](https://learn.microsoft.com/en-gb/fabric/data-warehouse/column-level-security), object-level security; [dynamic data masking](https://learn.microsoft.com/en-gb/fabric/data-warehouse/dynamic-data-masking)
  - Masking: workspace `Administrator, Member and Contributor` read masked columns **unmasked**
    - These roles carry `CONTROL` permission on the database
      - The workspace Administrator, Member and Contributor roles carry `CONTROL` permission on the database by design, and `CONTROL` includes UNMASK, so those users read masked columns unmasked no matter how the masks are defined. That is the shape of the symptom here: masking never restricts privileged users, so to hide these values from them the officer has to change the roles they hold, or pair masking with column-level and row-level security
  - RLS: **A Direct Lake semantic model**, when a query references a table that enforces row-level security, the model **falls back to DirectQuery mode**
      - > A Direct Lake semantic model normally answers queries from an in-memory cache of columns loaded straight from the Delta files, and that path cannot evaluate a security policy defined at the SQL analytics endpoint. So when a query references a table that enforces row-level security, the model falls back to DirectQuery mode and sends the query to the SQL analytics endpoint, which applies the filter predicate before returning rows.

- `CONTROL` is a very broad permission — it effectively grants full control over the object (and cascades downward through the hierarchy: `CONTROL` at the database level covers everything under it).

- `SCHEMABINDING` (views/functions, tied to security policy)
- Ranking functions: `ROW_NUMBER, RANK, DENSE_RANK, NTILE`
- Calling `awaitTermination()` keeps the continuous query running;
- [Warehouse snapshot](https://learn.microsoft.com/en-gb/fabric/data-warehouse/warehouse-snapshot "DW snapshot") (read-only, point-in-time)
  - read-only for every consumer regardless of workspace role, so even Members and Admins cannot run INSERT or other
- [Query Insights](https://learn.microsoft.com/en-gb/fabric/data-warehouse/query-insights "Query Insight"): `exec_requests_history, long_running_queries, frequently_run_queries, sql_pool_insights-view`
  - `queryinsights.exec_requests_history`
    - Per-query execution details such as allocated CPU time <br> and the amount of data scanned from memory, disk, and remote storage recorded

- DMVs:
  - What these answer: query-performance history, long-running queries, cache utilization, most CPU-intensive queries, user activity, SQL pool pressure/resources
  - Admin only: `sys.dm_exec_connections` (the only role that can run the `KILL` command)
  - All roles: `sys.dm_exec_sessions`, `sys.dm_exec_requests`
    - Member/Contributor/Viewer only see their own sessions/requests
  - `sys.dm_exec_requests` (the command, start time, and total elapsed time)
      - returns one row per active request, and its total_elapsed_time is the time elapsed since that request arrived
      - That view carries **no login name**, so a second step is needed to attribute it: `sys.dm_exec_sessions` holds login_name and is queried for the **session_id** found in the first step.
  - `sys.dm_exec_sessions`: holds login name

- [Statistics](https://learn.microsoft.com/en-us/fabric/data-warehouse/statistics)
- Warehouse optimization/performance [performance link](https://learn.microsoft.com/en-gb/fabric/data-warehouse/guidelines-warehouse-performance)
- Incremental copy / [Watermark](https://learn.microsoft.com/en-us/fabric/data-factory/tutorial-incremental-copy-data-warehouse-lakehouse), also see [ingestion](#data-factory--orchestration--ingestion)
- Identifying T-SQL errors

- [Restore in-place](https://learn.microsoft.com/en-gb/fabric/data-warehouse/restore-in-place): capability to restore a warehouse to a prior point in time from a restore point
- [Clone table](https://learn.microsoft.com/en-us/fabric/data-warehouse/clone-table): near-instant **zero-copy clone** (`CREATE TABLE ... AS CLONE OF`), current or past point-in-time; inherits constraints such as PK and UniqueKey & RLS & masking from the source; independent after creation (changes don't propagate in either direction)
  - > A read-only delta log is created for every table clone that is created within the Warehouse. The data files stored as delta parquet files are read-only.

<!-- ########################################## -->
# Real-Time Intelligence
- MS Learn link [fabric/real-time-intelligence](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/)
- separate KQL/Kusto link [kusto/?view=microsoft-fabric](https://learn.microsoft.com/en-us/kusto/?view=microsoft-fabric)
- Eventhouse: stores and queries high-volume real-time event data (KQL databases live inside it), [smart capacity control](https://learn.microsoft.com/en-gb/fabric/real-time-intelligence/eventhouse-smart-capacity-control)
  - Kusto DB [partitioning](https://learn.microsoft.com/en-us/kusto/management/partitioning-policy?view=microsoft-fabric): the default partition is based on `timestamp`: ingestion time, i.e. time of creation
- Eventstream: no-code ingest/transform/route — the other of the two streaming routes, see [Decision guides](#decision-guides)
  - [Configure settings for a Fabric eventstream](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/event-streams/configure-settings)
    - Event throughput setting: Low: < 10 MB/s, Medium: 10–100 MB/s, High: > 100 MB/s
- KQL basic syntax and queries
  - [best practices](https://learn.microsoft.com/en-us/kusto/query/best-practices?view=microsoft-fabric): put `where` first
  - Aggregation just uses `by`, not `group by`! -> `summarize avg(price) by category` — summarize is the KQL operator that aggregates, and the columns to group by follow the `by`. Also common to name the result, e.g. `summarize avgPrice = avg(price) by category`
- [Materialized views](https://learn.microsoft.com/fi-fi/kusto/management/materialized-views/materialized-view-create?view=microsoft-fabric) — when NOT to use them: one-off/rarely-run queries, non-SQL logic (ML/API/complex Python → use a Spark notebook instead), high-frequency streaming with sub-second needs (→ use Real-Time Intelligence as-is)
- Native tables vs. OneLake shortcuts in Real-Time Intelligence
- Query acceleration vs. a regular shortcut in Real-Time Intelligence
- [Fabric Activator](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/data-activator/activator-tutorial) (rule-based alerts/actions, [overview](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/data-activator/activator-rules-overview))
- [Real-Time Dashboards](https://learn.microsoft.com/en-gb/fabric/real-time-intelligence/dashboard-real-time-create)
- Windowing functions in a streaming context
- Identifying and optimizing Eventhouse/Eventstream errors
  - [Ingestion result logs](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/monitor-logs-ingestion-results):
    - IngestionErrorDetails provides the ingestion error,
    - ShouldRetry indicates whether the operation should be retried, and
    - IsIngestionOriginatesFromUpdatePolicy identifies failures caused by an update policy.
    - DurationMs, CapacityId, and OperationStartTime provide timing or placement context

# Mirroring
- MS Learn link [fabric/mirroring](https://learn.microsoft.com/en-us/fabric/mirroring/overview)
- Mirrored databases: Azure SQL DB, Cosmos DB, PostgreSQL, SQL Server, Snowflake, Databricks Unity Catalog
- Database mirroring vs. metadata mirroring vs. open mirroring
- SQL analytics endpoint for a mirrored database
- Also see the OneLake angle in the [OneLake](#onelake) section

# Workspace settings
- MS Learn link [fabric/fundamentals/workspaces](https://learn.microsoft.com/en-us/fabric/fundamentals/workspaces#workspace-settings)
- See also (skill area): [Configure Microsoft Fabric workspace settings](exam-domains.md#configure-microsoft-fabric-workspace-settings)
- Spark workspace settings (e.g. pool defaults)
- Domain workspace settings (associating a workspace with a domain)
- OneLake workspace settings
- Apache Airflow workspace settings (enable before creating jobs)

# Governance & Security
- MS Learn [fabric/security/security-overview](https://learn.microsoft.com/en-us/fabric/security/security-overview)
- Also see the [fabric/governance](https://learn.microsoft.com/en-us/fabric/governance/governance-compliance-overview) section
- See also (skill area): [Configure security and governance](exam-domains.md#configure-security-and-governance)
- Workspace-level vs. item-level access control
- Sensitivity labels (Purview integration)
- Endorsement: Promoted, Certified, Master data
- Fabric audit logs
- Domains as a governance concept (not an access-control mechanism)
- [Domains](https://learn.microsoft.com/en-us/fabric/governance/domains)
  - logically grouping together all the data in an organization that is relevant to a particular area or field.
  - To group data into domains, workspaces are associated with domains.

# Lifecycle management & CI/CD
- MS Learn [fabric/cicd/](https://learn.microsoft.com/en-us/fabric/cicd/cicd-overview)
- See also (skill area): [Implement lifecycle management in Fabric](exam-domains.md#implement-lifecycle-management-in-fabric)
- Version control: Git integration (Azure DevOps/GitHub)
- Database projects
- Deployment pipelines (dev/test/prod, deployment rules, variable libraries)
- CI/CD workflow options 1–4 (Git integration / Fabric Items APIs / deployment pipelines / CI-CD for ISVs), see [manage-deployment](https://learn.microsoft.com/en-us/fabric/cicd/manage-deployment#development-process) — only options 1 & 3 are explicitly on the exam
- Apache Airflow [Git integration](https://learn.microsoft.com/en-us/fabric/data-factory/apache-airflow-jobs-sync-git-repo)
  - > Data Workflows **only synchronizes** the `dags/` and `plugins/` folders from the repository. <br> Make sure your files or subfolders are inside one of these folders.

# Decision guides
- MS Learn [fabric/fundamentals/decision-guide](https://learn.microsoft.com/en-us/fabric/fundamentals/decision-guide-pipeline-dataflow-spark)
- Choosing the right data store: Lakehouse vs. Warehouse vs. Eventhouse vs. mirrored database (see [glossary.md](glossary.md))
- Choosing a transformation tool for batch data: Dataflow Gen2 vs. notebook (PySpark/SQL) vs. KQL vs. T-SQL
- Choosing an orchestration tool: Dataflow Gen2 vs. pipeline vs. notebook (a different skill from the previous one)
- Choosing a streaming engine — two distinct routes in Fabric: Spark Structured Streaming (notebook, e.g. writing to a Lakehouse Delta table) OR Eventstream → Eventhouse/KQL database (no-code, KQL query); the choice depends on the destination, latency requirements, and team skillset (Spark code vs. no-code)
- Designing a loading pattern for streaming data — the same split applies here too: Spark Structured Streaming into a Lakehouse, or the Eventstream/Eventhouse route
- Shortcut vs. Mirroring decision criteria (see the [OneLake section](#onelake))

# Administration & Capacity
- MS Learn [fabric/admin/](https://learn.microsoft.com/en-us/fabric/admin/admin-overview)
- **Admin** roles: Fabric/Tenant, capacity, domain, workspace (Entra ID: "Power BI Administrator")
- Tenant setting: "Users can create Fabric items"
- Capacity (F-SKU/CU), Capacity Metrics app
- Licenses: Fabric Free vs. Power BI Pro vs. PPU, the **F64 threshold**

# Monitoring & Optimization
- MS Learn [fabric/admin/monitoring](https://learn.microsoft.com/en-us/fabric/admin/monitoring-hub)
- **Monitoring hub**: ingestion, transformation, semantic model refresh
- Alerts
- Query Insights & DMVs (see the [Warehouse section](#warehouse))
- Identifying errors: pipeline, Dataflow Gen2, notebook, Eventhouse, Eventstream, T-SQL, OneLake shortcut
- Performance optimization: Lakehouse table, pipeline, warehouse, Eventstream/Eventhouse, Spark, queries
