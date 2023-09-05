# Developing monitor and status dashboards
## Azure Dashboards
**Azure dashboards** are the primary dashboarding technology for Azure. However, you can access data in log and metric data in Azure Monitor through their API using any REST client.

### Limitations of Azure Dashboards

- Limited control over log visualizations with no support for data tables. The total number of data series is limited to 10, with different data series grouped under another bucket.
- No custom parameters support for log charts.
- Log charts are limited to the last 30 days.
- Log charts can only be pinned to shared dashboards.
- No interactivity with dashboard data.
- Limited contextual drill-down.

## View designer in Azure Monitor
**View Designer in Azure Monitor** allows you to create custom visualizations with log data. They're used by monitoring solutions to present the data they collect.

### Limitations of view designer in Azure Monitor

- Supports logs but not metrics.
- No personal views. Available to all users with access to the workspace.
- No automatic refresh.
- Limited layout options.
- No support for querying across multiple workspaces or Application Insights applications.
- Queries are limited in response size to 8 MB and query execution time of 110 seconds.

## Azure Monitor workbooks
**Workbooks** are interactive documents that provide deep insights into your data, investigation, and collaboration inside the team. Specific examples where workbooks are helpful are troubleshooting guides and incident postmortem.

### Limitations of Azure Monitor workbooks

- No automatic refresh.
- No dense layout like dashboards, which make workbooks less useful as a single pane of glass. It's intended more for providing more profound insights.

## Power BI
**Power BI** is beneficial for creating business-centric dashboards and reports analyzing long-term KPI trends.
You can import the results of a log query into a Power BI dataset to take advantage of its features, such as combining data from different sources and sharing reports on the web and mobile devices.

## Limitations of Power BI

- It supports logs but not metrics.
- No Azure RM integration. Can't manage dashboards and models through Azure Resource Manager.
- Need to import query results need into the Power BI model to configure. Limitation on result size and refresh.