# Secure Azure VM Web Hosting: Production-Style Deployment

##  Project Purpose
In modern cloud environments, a "default" deployment is often an insecure one. This project was built to demonstrate a **Security-First** approach to cloud provisioning, moving beyond basic resource creation to implement enterprise-grade hardening.

The goal was to solve a common real-world problem: **How do we host a public-facing application without exposing the underlying infrastructure to unnecessary risk?**

##  The "Why" Behind the Design
This project addresses three critical security vulnerabilities found in standard cloud deployments:

* **Exposure of Management Ports:** Rather than leaving SSH open to the world (a magnet for botnets), I implemented **IP-restricted access** via Network Security Groups (NSGs).
* **Credential Leakage:** By enabling **System-Assigned Managed Identity**, I eliminated the need for hardcoded service principal secrets or API keys within the server configuration.
* **Lack of Perimeter Control:** I utilized a **Zero-Trust** networking model where all inbound traffic is denied by default, only explicitly allowing HTTP (80) for the application and SSH (22) for my specific IP.

##  Architecture & Traffic Flow
The environment is built using a modular approach inside a dedicated Azure Resource Group.



1.  **Internet Traffic:** Users access the web application via the Public IP over Port 80.
2.  **Firewall Layer (NSG):** The NSG inspects the packet. If it's HTTP, it's allowed; if it's SSH, it checks against my Trusted IP; everything else is dropped.
3.  **Compute Layer:** A hardened Linux VM running Nginx, utilizing a B1s SKU for optimal cost-efficiency.

##  Technical Stack
* **Cloud Provider:** Microsoft Azure
* **Infrastructure:** Virtual Network (VNet), Subnets, Public IP
* **Security:** Network Security Groups (NSGs), SSH Key-based Auth, Managed Identity
* **Web Server:** Nginx (Ubuntu Linux)

##  Key Skills Demonstrated
* **Cloud Security:** Implementing the Principle of Least Privilege (PoLP) at the network level.
* **Identity Management:** Configuring Azure RBAC and Managed Identities to remove static credentials.
* **Linux Administration:** Remote configuration and web server deployment via secure shell.
* **Governance:** Using standardized naming conventions (e.g., `rg-`, `vnet-`, `nsg-`) consistent with the Azure Cloud Adoption Framework.

##  Impact & Lessons Learned
This project serves as a template for a **production-ready landing zone**. It highlights the balance between **accessibility** (keeping the site live for users) and **security** (keeping the server invisible to attackers). 

The primary takeaway was the importance of "Defense in Depth"—ensuring that even if one layer is misconfigured, other controls (like Managed Identity and SSH Keys) continue to protect the environment.

---

**Status:**  Completed  
**Role:** Junior Cloud Engineer  
**Clean-up:** All resources deleted post-validation to maintain cost hygiene.
