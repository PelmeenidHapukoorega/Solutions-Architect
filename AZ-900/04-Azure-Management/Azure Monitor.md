# Describe Azure Monitor

## Table of contents
* [Azure Monitor](#azure-monitor)
* [Azure log Analytics](#azure-log-analytics)
* [Azure monitor alerts](#azure-monitor-alerts)
* [Alert security levels](#alert-security-levels)
* [Application insights](#application-insights)

## My interpretations

## Azure Monitor

Is a platform for collecting data on resources, analyzing that data, visualizing the info and acting on the results.

### Why does it exist?

To provide a centralized observability platform that helps you understand the performance and health of your entire IT stack in real time.

### What does it do?

It collects telemetry data, allows for deep log analysis, creates visualizations and triggers automated actions (like sending alerts or auto scaling).

### How does it work?

It gathers data into two main buckets: Metrics (numbers for speed) and Logs (text for detail). It then uses tools like Log Analytics to query that data.

### Summary

Its the Black Box of your cloud. It records everything happening inside your apps and servers so you can fix problems before users even notice them.

## Azure log Analytics

Is the tool  in the portal where you will write and run log queries on the data gathered by Azure Monitor.

### Why does it exist?

To provide a dedicated workspace where you can search, filter and analyze the massive amounts of raw log data collected by Azure Monitor.

### What does it do?

It runs Kusto Query Language (KQL) scripts to find specific events, perform statistical analysis and turn text based logs into visual charts.

### How does it work?

You type queries into an editor (like SQL), the tool retrieves data from the Log Analytics Workspace and displays it as tables or graphs.

### Summary

Its the Search Engine for your logs. If Azure Monitor is the bucket catching all the data, Log Analytics is the shovel you use to find the gold (errors or trends).

## Azure monitor alerts

Are an automated way to stay informed when Monitor detects a treshold is being crossed.

### Why does it exist?

To provide proactive notification when something goes wrong allowing you to fix issues before users report them.

### What does it do?

It monitors data (metrics or logs) checks it against your custom conditions and triggers Action Groups for notifications or automated fixes.

### How does it work?

You create an Alert Rule (the logic). When the signal hits a threshold (CPU > 80%) it sends a message via an Action Group (the delivery).

### Summary

It is your cloud Alarm System. You set the sensors (rules), and it decides whether to send a text, call a technician or try to fix the leak itself.

### Alert security levels

Azure categorizes alerts into 5 levels of urgency:

* Sev 0 (Critical): Immediate action required (service is down).
* Sev 1 (Error): Major issue.
* Sev 2 (Warning): Potential problem.
* Sev 3 (Informational): Standard update.
* Sev 4 (Verbose): Detailed logging.

## Application insights

Is a Monitor feature that monitors your web apps. Its capable of monitoring apps that are running in Azure, on prem or in different cloud environments.

### Why does it exist?

To provide Application Performance Management (APM), helping developers see exactly how their web apps are performing and how users are interacting with them.

### What does it do?

It tracks server side metrics (requests, CPU), client-side metrics (page load speeds), dependencies (database/API speed) and errors/exceptions.

### How does it work?

Via an SDK (added to code) or an Agent (installed on the server/platform). It then sends this telemetry to an Azure Monitor Log Analytics workspace.

### Summary

Its a Stethoscope for your app code. It tells you not just if the server is up, but exactly why a specific button click is slow or failing for a user.


### Use cases

Once its up and running you can use it to monitor array of information such as:

* Request rates, response times, and failure rates.
* Dependency rates, response times, and failure rates to show whether external services are slowing down performance.
* Page views and load performance reported by users browsers.
* AJAX calls from web pages, including rates, response times and failure rates.
* User and session counts.
* Performance counters from Windows or Linux server machines, such as CPU, memory, and network usage.
