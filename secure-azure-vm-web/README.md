# 01 - Secure Azure VM Web Hosting: Production-Style Deployment

## Project Purpose
In modern cloud environments, a "default" deployment is often an insecure one. This project demonstrates a **Security-First** approach to cloud provisioning, moving beyond basic resource creation to implement enterprise-grade hardening. 

The goal was to solve a common real-world problem: **How do we host a public-facing application without exposing the underlying infrastructure to unnecessary risk?**

## The "Why" Behind the Design
This project addresses three critical security vulnerabilities:
* **Exposure of Management Ports:** Rather than leaving SSH open to the world, I implemented **IP-restricted access** via Network Security Groups (NSGs), whitelisting only my specific public IP.
* **Credential Leakage:** By enabling a **System-Assigned Managed Identity**, I eliminated the need for hardcoded service principal secrets or API keys.
* **Lack of Perimeter Control:** I utilized a **Zero-Trust** networking model where all inbound traffic is denied by default, explicitly allowing only Port 80 (HTTP) and Port 22 (SSH).

## Architecture & Traffic Flow
The environment is built inside a dedicated Azure Resource Group using a modular approach:
* **Internet Traffic:** Users access the web application via the Public IP over Port 80.
* **Firewall Layer (NSG):** The NSG inspects packets; HTTP is allowed, while SSH is restricted to my Trusted IP (PUBLIC IP).
* **Compute Layer:** A hardened Linux VM running Nginx, utilizing a cost-efficient SKU (B1s or equivalent available size like B1ls/B2s).



## Technical Stack
* **Cloud Provider:** Microsoft Azure
* **Infrastructure:** Virtual Network (VNet), Subnets, Public IP
* **Security:** Network Security Groups (NSGs), SSH RSA Key-based Auth, Managed Identity
* **Web Server:** Nginx (Ubuntu Linux)

##  Challenges & Troubleshooting
* **Environment Constraints:** Encountered region-specific availability issues for the B1s VM size; successfully pivoted to an alternative available SKU to maintain project momentum.
* **Cross-Platform Connectivity:** Resolved a "No such file or directory" error when attempting to access local `.pem` keys from the browser-based Azure Cloud Shell. 
* **The Solution:** Migrated to a **local Windows PowerShell** workflow, utilizing the `-i` flag to point directly to the RSA private key for a successful secure handshake.

## Key Skills Demonstrated
* **Cloud Security:** Implementing the Principle of Least Privilege (PoLP) at the network level.
* **Identity Management:** Configuring Azure RBAC and Managed Identities to remove static credentials.
* **Linux Administration:** Remote configuration and Nginx deployment via secure shell.
* **Governance:** Using standardized naming conventions (e.g., `rg-`, `vnet-`, `nsg-`).

## Impact 
This project serves as a template for a **production-ready landing zone**. It highlights the balance between accessibility (keeping the site live) and security (keeping the server invisible to attackers). 


---

