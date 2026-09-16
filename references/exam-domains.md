---
title: DP-700 official skill areas
tags:
  - fabric
  - dp700
markmap:
  colorFreezeLevel: 3
  initialExpandLevel: 3
#   activeNode: { placement: 'center'}
---

# DP-700
Source: [Study Guide for Exam DP-700](https://learn.microsoft.com/credentials/certifications/resources/study-guides/dp-700)

"Skills measured as of July 21, 2026" — check with MCP whether this is still current before the exam.

Three areas, weighted evenly at 30–35 % each.
Product-area deep dive of the same content: `topic-by-product-area.md`.

## Implement and manage an analytics solution
<!-- this comment is just that wikilink -preview shows correctly in markmap -->

### Configure Microsoft Fabric workspace settings
- See also: [Workspace settings](topic-by-product-area.md#workspace-settings)
- Configure Spark workspace settings [ms learn](https://learn.microsoft.com/en-us/fabric/data-engineering/environment-manage-compute)
- Configure domain workspace settings [ms learn](https://learn.microsoft.com/en-us/fabric/governance/domains)
- Configure OneLake workspace settings [ms learn](https://learn.microsoft.com/en-us/fabric/onelake/onelake-diagnostics-overview)
- Configure Apache Airflow workspace settings [ms learn](https://learn.microsoft.com/en-us/fabric/data-factory/apache-airflow-jobs-workspace-settings)

### Implement lifecycle management in Fabric
- See also: [Lifecycle management & CI/CD](topic-by-product-area.md#lifecycle-management--cicd)
- Configure version control [ms learn](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/git-get-started)
- Implement database projects [ms learn](https://learn.microsoft.com/en-us/fabric/database/sql/source-control)
- Create and configure deployment pipelines [ms learn](https://learn.microsoft.com/en-us/fabric/cicd/deployment-pipelines/get-started-with-deployment-pipelines)

### Configure security and governance
- See also: [Governance & Security](topic-by-product-area.md#governance--security) (also: Warehouse — row/column-level security & masking, OneLake — OneLake security)
- Implement workspace-level and item-level access controls [ms learn](https://learn.microsoft.com/en-us/fabric/security/permission-model)
- Implement row-level, column-level, object-level, and folder/file-level access controls [ms learn](https://learn.microsoft.com/en-us/fabric/onelake/security/table-column-row-security)
- Implement dynamic data masking [ms learn](https://learn.microsoft.com/en-us/fabric/data-warehouse/dynamic-data-masking)
- Apply sensitivity labels to items [ms learn](https://learn.microsoft.com/en-us/fabric/fundamentals/apply-sensitivity-labels)
- Endorse items [ms learn](https://learn.microsoft.com/en-us/fabric/governance/endorsement-overview)
- Implement and use Microsoft Fabric audit logs [ms learn](https://learn.microsoft.com/en-us/fabric/admin/track-user-activities)
- Configure and implement OneLake security [ms learn](https://learn.microsoft.com/en-us/fabric/onelake/security/get-started-security)

### Orchestrate processes
- See also: [Data Factory](topic-by-product-area.md#data-factory--orchestration--ingestion) (also: Decision guides)
- Choose between Dataflow Gen2, a pipeline, and a notebook [ms learn](https://learn.microsoft.com/en-us/fabric/fundamentals/decision-guide-pipeline-dataflow-spark)
- Design and implement schedules and event-based triggers [ms learn](https://learn.microsoft.com/en-us/fabric/data-factory/pipeline-runs)
- Implement orchestration patterns with notebooks and pipelines, including parameters and dynamic expressions [ms learn](https://learn.microsoft.com/en-us/fabric/data-factory/parameters)

## Ingest and transform data
<!-- this comment is just that wikilink -preview shows correctly in markmap -->

### Design and implement loading patterns
- See also: [Data Factory](topic-by-product-area.md#data-factory--orchestration--ingestion) (also: Warehouse — dimensional model)
- Design and implement full and incremental data loads [ms learn](https://learn.microsoft.com/en-us/fabric/data-factory/tutorial-incremental-copy-data-warehouse-lakehouse)
- Prepare data for loading into a dimensional model [ms learn](https://learn.microsoft.com/en-us/fabric/data-warehouse/dimensional-modeling-load-tables)
- Design and implement a loading pattern for streaming data [ms learn](https://learn.microsoft.com/en-us/fabric/data-factory/decision-guide-data-integration)

### Ingest and transform batch data
- See also: [OneLake](topic-by-product-area.md#onelake) (also: Mirroring, Lakehouse, Warehouse, Decision guides)
- Choose an appropriate data store [ms learn](https://learn.microsoft.com/en-us/fabric/fundamentals/decision-guide-data-store)
- Choose between Dataflows Gen2, notebooks, KQL, and T-SQL for data transformation [ms learn](https://learn.microsoft.com/en-us/fabric/data-factory/decision-guide-data-integration)
- Create and manage OneLake shortcuts [ms learn](https://learn.microsoft.com/en-us/fabric/onelake/onelake-shortcuts)
- Implement mirroring [ms learn](https://learn.microsoft.com/en-us/fabric/mirroring/overview)
- Ingest data by using pipelines [ms learn](https://learn.microsoft.com/en-us/fabric/data-factory/copy-data-activity)
- Transform data by using PySpark, SQL, and KQL [ms learn](https://learn.microsoft.com/en-us/fabric/data-engineering/tutorial-lakehouse-data-preparation)
- Denormalize data [ms learn](https://learn.microsoft.com/en-us/fabric/data-warehouse/dimensional-modeling-load-tables)
- Group and aggregate data [ms learn](https://learn.microsoft.com/en-us/fabric/data-engineering/tutorial-lakehouse-data-preparation#transform-data-for-business-aggregates)
- Handle duplicate, missing, and late-arriving data [ms learn](https://learn.microsoft.com/en-us/fabric/data-engineering/structured-streaming-stateful-processing)

### Ingest and transform streaming data
- See also: [Real-Time Intelligence](topic-by-product-area.md#real-time-intelligence) (also: Lakehouse — Spark Structured Streaming, Decision guides)
- Choose an appropriate streaming engine [ms learn](https://learn.microsoft.com/en-us/fabric/data-factory/decision-guide-data-integration)
- Choose between native tables and OneLake shortcuts in Real-Time Intelligence [ms learn](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/onelake-shortcuts)
- Choose between Query acceleration for OneLake shortcuts and standard OneLake shortcuts in Real-Time Intelligence [ms learn](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/query-acceleration-overview)
- Process data by using Eventstreams [ms learn](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/event-streams/overview)
- Process data by using Spark structured streaming [ms learn](https://learn.microsoft.com/en-gb/fabric/data-engineering/structured-streaming-best-practices?tabs=python)
- Process data by using KQL [ms learn](https://learn.microsoft.com/en-us/kusto/query/?view=microsoft-fabric)
- Create windowing functions [ms learn](https://learn.microsoft.com/en-us/stream-analytics-query/windowing-azure-stream-analytics)

## Monitor and optimize an analytics solution
<!-- this comment is just that wikilink -preview shows correctly in markmap -->

### Monitor Fabric items
- See also: [Monitoring & Optimization](topic-by-product-area.md#monitoring--optimization)
- Monitor data ingestion [ms learn](https://learn.microsoft.com/en-us/fabric/admin/monitoring-hub)
- Monitor data transformation [ms learn](https://learn.microsoft.com/en-us/fabric/admin/monitoring-hub)
- Monitor semantic model refresh [ms learn](https://learn.microsoft.com/en-us/power-bi/connect-data/refresh-data)
- Configure alerts [ms learn](https://learn.microsoft.com/en-us/fabric/data-factory/create-alerts-for-pipeline-runs)

### Identify and resolve errors
- See also: [Monitoring & Optimization](topic-by-product-area.md#monitoring--optimization) (also: each product's own section)
- Identify and resolve pipeline errors [ms learn](https://learn.microsoft.com/en-us/fabric/data-factory/pipeline-troubleshoot-guide)
- Identify and resolve Dataflow Gen2 errors [ms learn](https://learn.microsoft.com/en-us/fabric/data-factory/dataflow-gen2-data-destinations-validation-rules)
- Identify and resolve notebook errors [ms learn](https://learn.microsoft.com/en-us/fabric/data-science/fabric-notebooks-troubleshooting-guide)
- Identify and resolve Eventhouse errors [ms learn](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/manage-monitor-eventhouse)
- Identify and resolve Eventstream errors [ms learn](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/event-streams/monitor)
- Identify and resolve T-SQL errors [ms learn](https://learn.microsoft.com/en-us/fabric/data-warehouse/troubleshoot-ingestion-errors)
- Identify and resolve OneLake shortcut errors [ms learn](https://learn.microsoft.com/en-us/fabric/onelake/security/troubleshoot-onelake-security-for-sql-analytics-endpoints)

### Optimize performance
- See also: [Monitoring & Optimization](topic-by-product-area.md#monitoring--optimization) (also: Warehouse, Lakehouse, Real-Time Intelligence)
- Optimize a Lakehouse table [ms learn](https://learn.microsoft.com/en-us/fabric/fundamentals/table-maintenance-optimization)
- Optimize a pipeline [ms learn](https://learn.microsoft.com/en-us/fabric/enterprise/optimize-capacity#compute-optimization-by-fabric-experience)
- Optimize a data warehouse [ms learn](https://learn.microsoft.com/en-gb/fabric/data-warehouse/guidelines-warehouse-performance)
- Optimize Eventstreams and Eventhouses [ms learn](https://learn.microsoft.com/en-us/fabric/real-time-intelligence/eventhouse-compute-observability)
- Optimize Spark performance [ms learn](https://learn.microsoft.com/en-us/fabric/data-engineering/spark-monitoring-best-practices)
- Optimize query performance [ms learn](https://learn.microsoft.com/en-us/fabric/data-warehouse/guidelines-warehouse-performance#query-performance)
