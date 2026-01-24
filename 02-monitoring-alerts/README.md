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

## Configuration Audit (Changes Implemented)

| Category | Component | Configuration Details |
| :--- | :--- | :--- |
| **Workspace** | `law-secure-web` | Centralized Log Analytics Workspace for telemetry storage. |
| **Alert 1** | `Alert-High-CPU` | **Severity 2 (Warning)**: Triggers when Average CPU > 80% over 5 mins. |
| **Alert 2** | `Alert-VM-Availability` | **Severity 0 (Critical)**: Triggers when VM Availability Metric = 0. |
| **Actions** | `ag-email-alerts` | Automated email routing to the administrator upon alert firing. |
| **Validation** | **KQL Query** | `search * | summarize count() by $table` verified `Heartbeat` and `Syslog` ingestion. |

##  Challenges & Troubleshooting
* **The "Silent" Agent:** Initially, the VM wasn't reporting detailed guest-level metrics.
  * **The Solution:** Identified that basic VM metrics only cover host-level data. I confirmed the **Azure Monitor Agent (AMA)** was correctly provisioned and linked to a **Data Collection Rule (DCR)** to begin streaming guest OS telemetry.
* **Alert Latency vs. Data Ingestion:** During simulation, I noted a delay between the crash and the notification.
  * **The Lesson:** Learned that stopping a VM halts the agent immediately, which can affect the final average calculation of a metric before an alert fires. This highlighted the importance of tuning **Evaluation Frequency** vs. **Lookback Periods**.

## Step 7: Incident Simulation & Validation
Testing ensures that the alert logic and notification channels are reliable before a real-world production crisis occurs.

### 1. CPU Stress Test (Performance)
* **Action:** Connected via **Native SSH** and executed `stress --cpu 2 --timeout 300` to pin CPU at 100%.
* **Result:** Azure Monitor detected the breach of the 80% threshold.
* **Outcome:** Successfully received an automated **Severity 2** email alert.

### 2. Availability Test (Uptime)
* **Action:** Performed a manual shutdown of the VM via the Azure Portal.
* **Result:** The system detected the loss of the agent heartbeat.
* **Outcome:** Successfully triggered a **Severity 0 (Critical)** alert in the portal dashboard.



## Impact & Lessons Learned
This project transitioned the environment into a **Production-Ready** state. The primary takeaway was that **monitoring is a feedback loop**: a metric without an alert is just noise, and an alert without an action is a distraction. I now have a validated framework for detecting and responding to infrastructure degradation.

**Status:** Completed  
**Role:** Junior Cloud Engineer  
**Clean-up:** Alerts disabled and logs cleared post-validation to maintain cost hygiene.
