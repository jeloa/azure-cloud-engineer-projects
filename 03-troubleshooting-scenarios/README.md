# 03 - Azure Infrastructure Troubleshooting: Post-Deployment Remediation

## Project Purpose
Cloud engineering is not just about building; it is about maintaining and repairing. This project shifts the focus from deployment to **Operations & Maintenance (O&M)**. I simulated four high-impact, real-world failure scenarios to demonstrate a systematic approach to cloud troubleshooting: **Detection → Diagnosis → Resolution → Validation**.

The goal was to prove technical resilience: demonstrating how to restore a production environment when network, storage, identity, or monitoring systems fail.

---

## Architecture & Lifecycle Model
The project follows a standard remediation lifecycle to ensure that every fix is documented and verified before being marked as resolved.



---

## Troubleshooting Scenarios

### **Scenario 1: Network Connectivity Failure (NSG)**
* **Failure**: Modified **Network Security Group (NSG)** rules to block inbound traffic on Port 80 (HTTP).
* **Symptoms**: Web server became unreachable via public IP; browser returned "Connection Timed Out".
* **Diagnosis**: Audited the **Inbound Security Rules** and identified a high-priority "Deny" rule overriding the web traffic allow rule.
* **Resolution**: Re-configured the NSG to **Allow** traffic on Port 80/8080 and adjusted priority levels.
* **Validation**: Confirmed the "Welcome to Nginx" page was accessible via the public IP.

### **Scenario 2: OS-Level Resource Exhaustion (Disk Full)**
* **Failure**: Saturated the root partition (`/dev/root`) by creating a 25GB dummy file via `fallocate`.
* **Symptoms**: `df -h` confirmed **99% capacity**; system reported "No space left on device," breaking logging and service updates.
* **Diagnosis**: Monitored **OS Disk Write Bytes/Sec** metrics in the Azure Portal to identify the I/O spike.
* **Resolution**: Reclaimed space by identifying and deleting the filler file using `sudo rm /tmp/disk_filler.img`.
* **Validation**: Confirmed storage returned to a healthy **21% usage** and system performance stabilized.

### **Scenario 3: Access Denied (RBAC Misconfiguration)**
* **Failure**: Assigned a user the **Reader** role, simulating an unauthorized or accidental permission change that restricts management actions.
* **Symptoms**: User received "Authorization Failed" errors when attempting to restart the VM; management buttons were greyed out.
* **Diagnosis**: Utilized the **Check access** tool in **Access Control (IAM)** to identify missing write permissions for "Test User One".
* **Resolution**: Reassigned the **Virtual Machine Contributor** role at the Resource Group scope.
* **Validation**: Verified via the IAM dashboard that administrative rights were restored.

### **Scenario 4: Monitoring Data Missing (Agent Failure)**
* **Failure**: Stopped the **Azure Monitor Agent (AMA)** service directly within the Linux OS.
* **Symptoms**: Total data gap in **Log Analytics**; heartbeats and guest-level performance metrics stopped updating.
* **Diagnosis**: Checked VM extensions and then reviewed service status via SSH using `systemctl status azuremonitoragent`.
* **Resolution**: Restarted the agent service and enabled it for automatic boot persistence.
* **Validation**: Executed a **KQL query** in Log Analytics to confirm heartbeat records resumed with current timestamps.

---

## Technical Stack & Tools
* **Cloud Platform**: Microsoft Azure
* **Monitoring & Analytics**: Azure Monitor, Log Analytics (KQL Queries), Metrics Explorer
* **Identity & Security**: Role-Based Access Control (RBAC), Network Security Groups (NSG)
* **Linux Administration**: SSH, Systemd (Service Management), Disk Utility Tools (`df`, `fallocate`)

## Key Skills Demonstrated
* **Incident Response**: Applying a structured methodology to resolve service-impacting issues.
* **Log Analysis**: Using Kusto Query Language (KQL) to verify system health and telemetry.
* **Identity Governance**: Auditing and fixing RBAC inheritance and assignment issues.
* **Performance Monitoring**: Correlating portal-side metrics with OS-side reality.

## Impact & Lessons Learned
This project underscores that **visibility is the first step of security**. Without working monitoring (Scenario 4) or correct permissions (Scenario 3), a cloud environment is unmanageable. I learned to move beyond "restarting the VM" and instead look for root causes, whether it's a specific NSG priority number or a specific directory consuming disk space.


