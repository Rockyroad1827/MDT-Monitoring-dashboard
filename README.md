# Live MDT Monitoring Web Dashboard

![MDT](https://img.shields.io/badge/MDT-Microsoft%20Deployment%20Toolkit-blue.svg) ![IIS](https://img.shields.io/badge/Server-IIS-orange.svg)

This guide details the complete process for setting up a modern, real-time web dashboard to monitor your Microsoft Deployment Toolkit (MDT) deployments. The dashboard connects directly to the MDT monitoring web service via an IIS reverse proxy, providing a live look at deployments in progress from a standard web port.

### Final Result Overview
![Dashboard Screenshot](https://github.com/user-attachments/assets/e336794e-318d-45de-b1a1-d02c2af13ba0)

---

## Prerequisites

Before you begin, ensure your environment meets these requirements:

* ✅ A server running MDT (e.g., `Server1`).
* ✅ The **MDT Monitoring Service** feature enabled in MDT (typically running on port `9801`).
* ✅ The **Internet Information Services (IIS)** role installed on the same server.
* ✅ **Administrator access** to the server.

---

## Part 1: Install IIS Add-ons

To bridge data from the MDT service (port `9801`) to standard web traffic (port `80`), we must configure IIS to act as a reverse proxy. This requires two specific Microsoft add-ons.

### 1. Application Request Routing (ARR) 3.0
This module enables IIS to handle request forwarding and proxying.
> [Download ARR 3.0](https://www.iis.net/downloads/microsoft/application-request-routing)

### 2. URL Rewrite
This module allows us to define rules that intercept incoming traffic and redirect it to the MDT port.
> [Download URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite)

> **⚠️ Important:** After installing both modules, you must close and reopen the **IIS Manager** console for the new features to appear in the UI.

---

## Part 2: Configure IIS

### Step 2.1: Create the Website Folder and Site
1.  On your MDT server, create a physical folder to host the dashboard files:
    `C:\inetpub\wwwroot\MDTMonitoringDashboard`
2.  Open **IIS Manager**.
3.  In the **Connections** pane (left side), right-click **Sites** and select **Add Website...**.
4.  Configure the site details:
    * **Site name:** `MDT Monitoring Dashboard`
    * **Physical path:** Browse to the folder created above.
    * **Port:** `80` (use `8080` if 80 is already occupied).
    * **IP address:** Select your server's specific IP (e.g., `192.168.1.3`).
5.  Click **OK**.

### Step 2.2: Enable Server-Level Proxying
1.  In IIS Manager, click the root server node in the **Connections** pane (e.g., `Server1`).
2.  In the center feature view, locate and double-click **Application Request Routing Cache**.
3.  In the **Actions** pane (far right), click **Server Proxy Settings...**.
4.  Check the top box labeled **Enable proxy**.
5.  Leave default settings below and click **Apply**.

---

## Part 3: Add Dashboard Files

Navigate to your website folder (`C:\inetpub\wwwroot\MDTMonitoringDashboard`) and create the following two necessary files. Or clone the git reprositry
