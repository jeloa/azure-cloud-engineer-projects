# Project 4: Azure Cost Management & Optimization (Business-Aware Cloud Engineering)

## Overview

This project focuses on **cost visibility, control, and optimization** — a critical but often overlooked responsibility of Junior Cloud Engineers.

The goal is to demonstrate that cloud resources are not only deployed and operated correctly, but also **managed responsibly from a cost and governance perspective**.

This project simulates how engineers identify waste, control spend, and apply optimizations in real environments.

---

## Objectives

* Understand Azure billing and cost structure
* Analyze resource-level costs
* Identify inefficient or unnecessary spend
* Apply cost optimization techniques
* Implement basic governance controls

---

## Architecture

**Components Used:**

* Azure Cost Management
* Azure Budgets
* Azure Virtual Machine
* Resource Groups
* Azure Tags

**Cost Flow:**

* Resource usage → Metered billing → Cost analysis → Optimization


---

## Step-by-Step Implementation (Mentor-Guided)

### Step 1: Review Cost Management Overview

**Why this matters:** Engineers must understand where money is being spent.

1. Go to **Azure Portal** → **Cost Management + Billing**
2. Select your **Subscription**
3. Open **Cost analysis**
4. Set scope to your project Resource Group

![Cost Analysis](screenshots/Cost-analysis.jpg)

---

### Step 2: Identify Cost Drivers

**Why this matters:** Optimization starts with visibility.

1. In **Cost analysis**, group by:

   * Service name
   * Resource
2. Identify:

   * VM compute costs
   * Disk storage costs
   * Public IP / network costs

![Cost Breakdown](screenshots/Resource.jpg)

---

### Step 3: Right-Size the Virtual Machine

**Why this matters:** Over-provisioned VMs are one of the most common cost issues.

1. Go to **Virtual machines** → Select VM
2. Review:

   * CPU utilization (Azure Monitor)
   * Memory usage (if available)
3. Resize VM to a smaller SKU if utilization is consistently low



---

### Step 4: Configure Auto-Shutdown

**Why this matters:** Non-production workloads should not run 24/7.

1. Go to **Virtual machine** → **Auto-shutdown**
2. Enable auto-shutdown
3. Set time (e.g., 7:00 PM local time)
4. Configure notification email



---

### Step 5: Apply Resource Tags

**Why this matters:** Tags enable cost tracking and accountability.

Apply the following tags to all project resources:

* `Environment: Dev`
* `Project: Azure-Cloud-Portfolio`
* `Owner: <YourName>`



---

### Step 6: Create a Budget and Alert

**Why this matters:** Budgets prevent unexpected spending.

1. Go to **Cost Management + Billing** → **Budgets**
2. Create a budget:

   * Scope: Resource Group
   * Amount: Small monthly limit (e.g., $5–$10)
   * Alert: 80% threshold
3. Configure email notification



---

## Validation Checklist

* Cost data visible by resource
* VM utilization reviewed
* Auto-shutdown enabled
* Tags applied consistently
* Budget alert configured

---

## Key Skills Demonstrated

* Azure Cost Management analysis
* Resource optimization and right-sizing
* Budgeting and spend control
* Cloud governance fundamentals

---

## Insights

> Analyzed Azure resource costs using Cost Management, implemented VM right-sizing and auto-shutdown policies, applied tagging for governance, and configured budget alerts to control cloud spending.

---

## Lessons Learned

* Cost visibility is essential for sustainable cloud operations
* Small configuration changes can significantly reduce spend
* Engineers share responsibility for cloud costs

---

## Status

 Completed – Cost controls and optimizations implemented

