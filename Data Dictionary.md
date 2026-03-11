# Data Dictionary: Azure Cloud Cost & AI Workload Optimization

This document outlines the data structure used for the Azure Cloud Compute & AI Workload Optimization Power BI dashboard. It includes both the raw telemetry data extracted from the source CSV and the engineered columns created via Power Query to simulate Azure billing and AI-specific workloads.

## 1. Raw Telemetry Data (Source)

| Column Name | Expected Data Type | Description | Dashboard Usage |
| :--- | :--- | :--- | :--- |
| `vm_id` | Text | Unique alphanumeric UUID for each virtual machine. | Primary key linking performance data to specific hardware on the Decomposition Tree. |
| `timestamp` | Date/Time | The exact date and time the telemetry was recorded. | Split in Power Query into `Date` (for forecasting) and `Hour` (for peak-usage heatmaps). |
| `cpu_usage` | Decimal | Percentage of CPU capacity utilized. | Primary metric used to identify idle "Zombie Servers" (instances running <15%). |
| `memory_usage` | Decimal | Percentage of RAM utilized. | Visualized as bubble size on the scatter plot to indicate the scale of idle resources. |
| `network_traffic` | Decimal | Volume of data transferred in and out. | Contextual telemetry for network bandwidth analysis. |
| `power_consumption` | Decimal | Energy footprint of the VM (watts). | Secondary metric for hardware efficiency analysis. |
| `num_executed_instructions` | Whole Number | Raw volume of processing tasks handled. | Validates whether a server is actively computing versus spinning idle. |
| `execution_time` | Decimal | Duration the specific task ran. | Multiplied by the fabricated hourly rate to calculate actual billing costs. |
| `energy_efficiency` | Decimal | Derived ratio of instructions processed per watt. | Secondary metric for tracking hardware performance. |
| `task_type` | Text | Nature of the workload (`compute`, `io`, `network`). | Used to conditionally map generic servers to specific Azure hardware tiers. |
| `task_priority` | Text | Urgency of the workload (`low`, `medium`, `high`). | Combined with task type to assign costs to specific engineering workloads. |
| `task_status` | Text | Current state of the job (`waiting`, `running`, `completed`). | Used to filter out inactive/waiting tasks from the active cost calculation. |

## 2. Engineered Columns (Power Query Transformations)

These columns are synthesized within Power BI to map the raw telemetry directly to enterprise cloud economics and specific project architectures.

| Engineered Column | Data Type | Logic / Transformation Rule | Purpose |
| :--- | :--- | :--- | :--- |
| `Azure_Hardware_Tier` | Text | `if [task_type] = "compute" then "Azure NC A100 (GPU)" else if [task_type] = "io" then "Azure D-Series" else "Azure B-Series"` | Maps generic telemetry to specific Microsoft Azure pricing tiers for realistic billing. |
| `Assigned_Workload` | Text | `if [task_type] = "compute" and [task_priority] = "high" then "MirrorDB Synthesis" else if [task_type] = "compute" and [task_priority] = "medium" then "AQI Deep Learning Model" else "General IT"` | Assigns server usage directly to specific data generation and AI engineering projects. |
| `Calculated_Cost` | Currency ($) | `[execution_time] * [Fabricated_Hourly_Rate]` | Generates the core financial metric used across all KPI cards, scatter plots, and forecasting visuals. |
