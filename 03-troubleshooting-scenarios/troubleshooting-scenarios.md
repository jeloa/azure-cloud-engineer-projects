# Project 3: Azure Troubleshooting & Root Cause Analysis (Production Simulation)

## Overview

This project focuses on **troubleshooting, investigation, and root cause analysis (RCA)** — core responsibilities of a Junior Cloud Engineer supporting live environments.

Unlike deployment projects, this project intentionally introduces **failures and misconfigurations** and requires diagnosing issues using Azure-native tools, logs, and systematic thinking.

This simulates real on-call and support scenarios where engineers must restore service quickly and safely.

---

## Objectives

* Practice systematic troubleshooting of Azure infrastructure issues
* Identify root causes using logs, metrics, and configuration review
* Restore service with minimal impact
* Document findings in a professional RCA format

---

## Architecture

**Components Used:**

* Azure Virtual Machine (Linux)
* Azure Virtual Network & Subnet
* Network Security Group (NSG)
* Azure Monitor & Log Analytics

**Scenario Model:**

* Healthy state → Failure introduced → Detection → Diagnosis → Resolution → Validation

*(Insert Troubleshooting Flow Diagram Here)*

---

## Troubleshooting Methodology (Used for All Scenarios)

1. Identify symptoms (what is broken)
2. Check monitoring alerts and metrics
3. Review recent configuration changes
4. Isolate the failing component
5. Apply fix
6. Validate service restoration
7. Document root cause

This structured approach mirrors real production incident handling.

---

## Scenario 1: Web Application Unreachable (NSG Misconfiguration)

### Problem Introduced

Inbound HTTP traffic is blocked due to an incorrect NSG rule.

### Symptoms

* Website fails to load via public IP
* VM is running and reachable via SSH

### Investigation Steps

* Verify VM status (Running)
* Review NSG inbound rules
* Confirm HTTP (port 80) is missing or denied

### Root Cause

NSG inbound rule for HTTP traffic was removed or incorrectly prioritized.

### Resolution

* Add or correct NSG rule allowing TCP port 80
* Ensure rule priority allows traffic

### Validation

* Refresh browser
* Confirm web page loads successfully

📸 Screenshot: NSG rule fix and restored web page

---

## Scenario 2: High Disk Usage Causing Service Failure

### Problem Introduced

Disk fills up due to large files created intentionally.

### Symptoms

* Application becomes unresponsive
* System logs show disk-related errors

### Investigation Steps

* Check VM metrics (Disk Used Percentage)
* Review system logs via SSH

### Root Cause

Insufficient disk space available for application operations.

### Resolution

* Remove unnecessary files
* (Optional) Resize disk

### Validation

* Confirm disk usage returns to normal
* Verify service stability

📸 Screenshot: Disk metrics before and after cleanup

---

## Scenario 3: Access Denied (RBAC Misconfiguration)

### Problem Introduced

User loses access to VM or resource group.

### Symptoms

* Azure Portal shows authorization errors

### Investigation Steps

* Review role assignments at Resource Group and VM level
* Identify missing or incorrect role

### Root Cause

RBAC role removed or incorrectly assigned.

### Resolution

* Reassign correct role (e.g., Virtual Machine Contributor)

### Validation

* Confirm access restored

📸 Screenshot: RBAC role correction

---

## Scenario 4: Monitoring Data Missing (Agent Failure)

### Problem Introduced

Monitoring agent is stopped or uninstalled.

### Symptoms

* No new data in Log Analytics
* Alerts stop triggering

### Investigation Steps

* Check VM extensions
* Review agent status

### Root Cause

Monitoring agent stopped or misconfigured.

### Resolution

* Restart or reinstall monitoring agent

### Validation

* Confirm logs and metrics resume

📸 Screenshot: Log Analytics data restored

---

## Root Cause Analysis (Sample Format)

**Incident:** Web Application Outage

**Impact:** Web service unavailable to users

**Root Cause:** NSG inbound rule misconfiguration blocking HTTP traffic

**Resolution:** Corrected NSG rule allowing TCP port 80

**Prevention:** Implemented configuration review and monitoring alerts

---

## Key Skills Demonstrated

* Incident investigation
* Root cause analysis
* Azure networking and security troubleshooting
* Log and metric analysis
* Service restoration

---

## Resume Bullet (Ready to Use)

> Diagnosed and resolved multiple Azure infrastructure incidents by analyzing logs, metrics, and configuration errors, performing root cause analysis, and restoring service availability.

---

## Lessons Learned

* Small configuration changes can cause major outages
* Monitoring accelerates troubleshooting
* Structured RCA improves response time

---

## Status

🟡 In Progress – Troubleshooting scenarios executed
