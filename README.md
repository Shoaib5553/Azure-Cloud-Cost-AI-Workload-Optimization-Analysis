# Azure Cloud Cost & AI Workload Optimization

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

## 📌 Project Overview
Developed an enterprise-grade Power BI dashboard focuses on Cloud FinOps and infrastructure optimization. The primary objective is to ingest raw server telemetry, simulate Azure billing environments, and identify "Zombie Servers" (idle compute instances wasting financial resources). 

A core focus of this diagnostic tool is isolating the heavy compute costs associated with Artificial Intelligence workloads, specifically tracking high-tier GPU usage for projects like the **MirrorDB Synthetic Data Engine** and **AQI Deep Learning Models**.

## 🛠️ Tech Stack
* **BI Tool:** Power BI Desktop
* **Data Engineering:** Power Query (M Code) for ETL and data synthesis
* **Analytics:** DAX (Data Analysis Expressions) for What-If scenario modeling and time-intelligence
* **Data Modeling:** Star Schema architecture

## 📊 Dashboard Architecture & Features

The project is structured into four highly focused, interactive report pages:

1. **Executive Overview:** High-level FinOps KPIs (Total Spend, Wasted Spend, Avg CPU Utilization) and a 30-day automated cost forecast using Power BI's native predictive analytics.
2. **AI & GPU Workload Tracker:** A Decomposition Tree allowing managers to drill down from total cloud spend directly into specific AI projects, Azure hardware tiers, and individual Virtual Machine IDs.
3. **Zombie Server Diagnostics:** A diagnostic scatter plot identifying high-cost, low-utilization servers. Features a dynamic **What-If Parameter** that calculates real-time financial savings if servers running below 15% CPU are shut down.
4. **Peak Hours Heatmap:** A day-by-hour matrix visualizing exact server utilization trends, providing the data-backed justification needed to schedule automated off-peak server shutdowns.

## 🗄️ Data Engineering & Star Schema
To simulate a real-world Azure billing environment, raw cloud telemetry (CPU, Memory, Execution Time) was transformed via Power Query. 
* **Data Synthesis:** A custom Azure Rate Card was engineered to map generic `task_types` to specific Microsoft hardware (e.g., Azure NC A100 GPUs, Azure D-Series).
* **Workload Mapping:** Compute costs were programmatically assigned to specific CSE AI/ML engineering workloads.
* **Relational Model:** The flat CSV was normalized into a highly efficient **Star Schema** featuring a central `Fact_Telemetry` table surrounded by `Dim_Hardware`, `Dim_Workload`, and `Dim_Date` tables.


## 🚀 How to Run the Project
1. Download the `.pbix` file from this repository.
2. Open the file using **Power BI Desktop** (Free).
3. To test the security architecture, navigate to the **Modeling** tab, click **View as**, and select the `AI_Engineering_Lead` role.
4. Navigate to the **Zombie Server** page to interact with the What-If savings slider.

## 📄 License
This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.
