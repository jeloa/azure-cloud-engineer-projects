# 02 - Azure Operational Readiness: Monitoring, Logging & Incident Response

## Project Purpose
Deploying a resource is only half the battle; maintaining its uptime is the other. This project shifts the focus from **deployment** to **operations**. I established a centralized "command center" for the infrastructure created in Project 1, ensuring that system health is measurable and that failures trigger immediate, automated responses.

The goal was to solve a critical operational problem: **How do we move from reactive "firefighting" to proactive infrastructure management?**

## The "Why" Behind the Design
This project implements three pillars of modern Site Reliability Engineering (SRE):
* **Centralized Observability:** By routing all data to a **Log Analytics Workspace (LAW)**, I eliminated "data silos," allowing for complex cross-resource querying using **KQL**.
* **Automated Incident Response:** Rather than manually checking CPU graphs, I configured **Action Groups** to provide instant notification, reducing the Mean Time to Detection (MTTD).
* **Validation through Chaos:** I didn't assume the monitoring worked; I performed **Failure Injection** (CPU stress and manual shutdowns) to prove the alerting pipeline was functional.

## Architecture & Data Flow
The monitoring ecosystem follows a structured telemetry pipeline:



1. **Data Ingestion:** The **Azure Monitor Agent (AMA)** collects OS-level performance metrics and system logs from the VM.
2. **Aggregation:** Data is streamed into the **Log Analytics Workspace (law-secure-web)** for storage and analysis.
3. **Evaluation:** **Azure Monitor** runs periodic checks against the stored data.
4. **Notification:** If thresholds are breached (e.g., CPU > 80% or VM Heartbeat = 0), the **Action Group** triggers an email notification.

## Technical Stack
* **Cloud Provider:** Microsoft Azure
* **Observability:** Azure Monitor, Log Analytics Workspace (LAW)
* **Query Language:** Kusto Query Language (KQL)
* **Automation:** Azure Monitor Alerts, Action Groups (Email)
* **Testing Tools:** Linux `stress` utility

## 🛠️ Challenges & Troubleshooting
* **The "Silent" Agent:** Initially, the VM wasn't reporting detailed guest-level metrics.
  * **The Solution:** Identified that basic VM metrics only cover host-level data. I enabled **VM Insights** and confirmed the **Azure Monitor Agent (AMA)** was correctly provisioned and linked to a Data Collection Rule (DCR).
* **Alert Latency:** During the CPU stress test, the email didn't arrive instantly.
  * **The Lesson:** Learned the difference between **Evaluation Frequency** (how often Azure looks at the data) and **Lookback Period** (the time window analyzed). I tuned the alert to check every 1 minute to speed up response times for critical failures.

## Key Skills Demonstrated
* **Log Analytics & KQL:** Writing custom queries to filter and visualize performance data.
* **Operational Health:** Defining meaningful thresholds to avoid "Alert Fatigue."
* **Incident Simulation:** Using stress-testing tools to validate production readiness.
* **Resource Governance:** Ensuring monitoring costs are managed by targeting only essential telemetry.


## Impact & Lessons Learned
This project transitioned the environment into a **Production-Ready** state. The primary takeaway was that **monitoring is a feedback loop**: a metric without an alert is just noise, and an alert without an action is a distraction. I now have a validated framework for detecting and responding to infrastructure degradation.

**Status:** Completed  
**Role:** Junior Cloud Engineer  
**Clean-up:** Alerts disabled and logs cleared post-validation to maintain cost hygiene.

##  Future Enhancements
* **Self-Healing:** Configure an **Azure Automation Runbook** to automatically restart the Nginx service if it stops.
* **Infrastructure as Code (IaC):** Deploy the entire monitoring stack (LAW, Alerts, DCRs) using Bicep or Terraform.
