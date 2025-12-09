Creating a Live MDT Monitoring Web Dashboard
This guide details the complete process for setting up a modern, real-time web dashboard to monitor your Microsoft Deployment Toolkit (MDT) deployments. The dashboard connects directly to the MDT monitoring web service, providing a live look at deployments in progress from a simple web page.

This is the final result:

Prerequisites
Before you begin, ensure you have the following:

A server running MDT (e.g., MB03).

The MDT Monitoring Service enabled and running (this is a feature of MDT, typically on port 9801).

The Internet Information Services (IIS) role installed on the same server.

Administrator access to the server.

Part 1: Install IIS Add-ons
To get data from the MDT service (on port 9801) to our website (on port 80), we must install two add-ons for IIS that allow it to act as a reverse proxy.

Install Application Request Routing (ARR):

This module allows IIS to forward requests.

Download Link: Application Request Routing (ARR) 3.0

Install URL Rewrite:

This module reads our rules to know which requests to forward.

Download Link: URL Rewrite

After installing both, you must close and re-open IIS Manager for the new icons to appear.

Part 2: Configure IIS
Step 2.1: Create the Website
On your server, create a new folder for the website files (e.g., C:\inetpub\wwwroot\MDTMonitoringDashboard).

Open IIS Manager.

In the "Connections" pane, right-click Sites and select "Add Website...".

Fill out the details:

Site name: MDT Monitoring Dashboard

Physical path: C:\inetpub\wwwroot\MDTMonitoringDashboard (or the folder you just created).

Binding > Port: 80 (or another port like 8080 if 80 is in use).

Binding > IP address: Select your server's IP (e.g., 192.168.1.3).

Click OK.

Step 2.2: Enable the IIS Proxy (Server Level)
In IIS Manager, click your server's name in the "Connections" pane (e.g., MB03).

In the center "Home" pane, find and double-click the "Application Request Routing Cache" icon.

In the "Actions" pane (far right), click "Server Proxy Settings...".

Check the box for "Enable proxy".

Click "Apply".

Part 3: Add the Dashboard Files
Navigate to the physical path you created (C:\inetpub\wwwroot\MDTMonitoringDashboard) and create the following two files.

 
Open the Deployment Workbench on your MDT server.

Right-click your Deployment Share and select "Properties".

Go to the "Rules" tab.

Add the following line to your [Settings] or [Default] section:

1
EventService=http://mb03:9801
Click OK or Apply.

CRITICAL STEP: You must now Update your Deployment Share.

Right-click your Deployment Share and select "Update Deployment Share".

Choose the option to completely regenerate your boot images.

Once complete, copy your new boot images (in the \Boot folder of your share) over to your WDS server to replace the old ones.
