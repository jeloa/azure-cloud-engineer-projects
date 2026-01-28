# 04 - Azure Cost Management & Optimization: Business-Aware Cloud Engineering

## Project Purpose
In a professional cloud environment, an engineer's job isn't just to make things *work*—it’s to make them **cost-efficient**. This project shifts the focus from deployment to **Financial Operations (FinOps)**. 

The goal was to simulate a real-world scenario where a Cloud Engineer identifies waste, enforces accountability through tagging, and prevents "bill shock" via automated governance and proactive budgeting.

## The "Why" Behind the Design
This project addresses the most common causes of "Cloud Waste":
* **Over-Provisioning:** Using **Azure Monitor** metrics to identify that my VM was 95% idle, then down-sizing the SKU to match actual demand.
* **Idle Resources:** Implementing **Auto-Shutdown** schedules to ensure non-production workloads don't rack up costs overnight.
* **Lack of Visibility:** Creating **Budgets** and **Alerts** so that spending is never a surprise, but a managed metric.
* **Governance Gaps:** Using **Resource Tags** to ensure every dollar spent is traceable to a specific project and owner.

## Architecture & Cost Flow
The project follows a circular FinOps lifecycle:
1.  **Resource Usage:** Monitoring active VM compute and storage disks.
2.  **Metered Billing:** Usage data is processed by the Azure billing engine.
3.  **Cost Analysis:** Grouping costs by **Resource Group** and **Tags** to identify "Cost Drivers."
4.  **Optimization:** Taking action through resizing, auto-shutdown, and budget alerts.

## Technical Stack
* **Cost Management:** Azure Budgets, Cost Analysis, Azure Advisor
* **Governance:** Resource Tags, Resource Groups
* **Compute Optimization:** VM Resizing (SKU selection), Auto-shutdown policies
* **Monitoring:** Azure Monitor (CPU Utilization Metrics)

## Challenges & Troubleshooting
* **Low-Utilization Discovery:** Upon reviewing the **Percentage CPU** metric, I discovered an average utilization of only **4.5%** over a 72-hour period. This provided the data-driven justification needed to downsize the VM.
* **Regional SKU Availability:** I initially found that the ultra-low-cost B-Series SKUs were restricted in my region. 
    * **The Solution:** I performed a **Stop (Deallocate)** on the VM to release the hardware lease. This refreshed the available SKU list, allowing me to successfully pivot to a more cost-effective size.
* **Data Latency:** Azure Cost Management data can lag by 8–24 hours. 
    * **The Solution:** I relied on real-time **Azure Monitor** metrics to make optimization decisions immediately rather than waiting for the next day's billing report.

## Key Skills Demonstrated
* **FinOps Analysis:** Identifying underutilized resources and calculating potential savings.
* **Resource Optimization:** Implementing "Scheduled Compute" via **Auto-Shutdown** to reduce waste by ~30% (8 hours/day).
* **Cloud Governance:** Implementing a standardized tagging strategy (`Environment: Dev`, `Project: Azure-Cloud-Portfolio`).
* **Budget Management:** Configuring automated **Budget Alerts** at 80% and 100% thresholds for proactive spend control.

## Impact & Lessons Learned
This project highlights the **Principle of Frugality**. By resizing an underutilized VM and enabling auto-shutdown, I demonstrated how to reduce a project's burn rate by over **50%** without affecting performance. 

