# Describe Microsoft Defender for Cloud

## Table of contents
## Table of contents

## Table of contents

* [Microsoft Defender for cloud](#microsoft-defender-for-cloud)
* [Protection everywhere you are deployed](#protection-everywhere-you-are-deployed)
* [Azure native protections](#azure-native-protections)
* [Defending hybrid resources](#defending-hybrid-resources)
* [Defending resources on other clouds](#defending-resources-on-other-clouds)
* [Assess, secure and defend](#assess-secure-and-defend)
* [Continuosly assess](#continuosly-assess)
* [Secure](#secure)
* [Defend](#defend)
* [Advanced threat protection](#advanced-threat-protection)





Microsoft Defender for cloud
Protection everywhere you are deployed
Azure native protections
Defending hybrid resources
Defending resources on other clouds
Assess, secure and defend
Continuosly assess
Secure
Defend
Advanced threat protection
## My interprentations

## Microsoft Defender for cloud

Is a monitoring tool for security posture management and threat protection.

### Why does it exist?

To solve security challenges created by modern cloud computing, which are too complex and fast moving for traditional security 
tools to handle. Its a unified intelligent security platform designed to address 3 core problems across Azure, AWS, GCP and hybrid environments. The core problems being: Cloud security posture management, Cloud workload protection platform, hybrid and multi cloud complexity.

### What does it do?

It acts as the central security brain for your entirecloud and hybrid environment. Primarily it does 2 essential things: Proactive prevention and active threat detection. Its often described as a cloud native app protection platform.

### How does it work?

It works by integrating deeply with your cloud platforms whether its Azure, AWS or GCP as well as hybrid environments to operate in 2 main security phases: Continuous assessment and real time protection.

### Summary

MDC works by continuously monitoring your setup to prevent problems and actively defending your running software to detect and respond to threats that makes it past the proactive checks. Essentially what it doesnt like it either removes instantly or puts it into quarantine for you to check if the threat is a concern or not.

## Protection everywhere you are deployed

* If you have on prem data center or operating in another cloud environment at the same time, monitoring of Azure services may not give you a complete picture of your security situation.
* MDC can deploy a log analytics agent to gather security related data.
* For Azure deployment is handled directly.
* For hybrid and multi cloud, MDC plans are extended to non Azure machines with the help of Azure Arc.
* CSPM features are extended to multicloud machines without the need for any agents.

## Azure native protections

MDC helps you detect threats across:

* PaaS services: Detects threats targeting Azure app service, Azure SQL, Azure Storage account and more. You can also perform anomaly detection within Azure activity logs using the native integration with MDC for cloud apps.
* Azure data services: MDC has capabilities that help you automatically classify data in Azure SQL. You can also get assessments for potential vulnerabilities across Azure SQL and Storage services as well as recommendations for how to mitigate them.
* Networks: MDC helps you limit exposure to brute force attacks. It reduces access to VM ports using the just in time VM access. You can harden network by preventing unnecessary access. Set secure access policies or selected ports for authorized users only, allow source IP address ranges or IP addresses and for a limited amount of time.

## Defending hybrid resources

* You can add MDC capabilities to your hybrid cloud environment to protect non Azure servers.
* Helps you focus on what matters the most.
* Youll get customized threat intelligence and prioritized alerts according to your specific environment.
* To extend protection to on prem machines, deploy Azure Arc and enable MDCs enhanced security features.

## Defending resources on other clouds

MDC can protect resources in other clouds both AWS and GCP.

Example: If you have connected AWS account to Azure sub you can enable the following protections:

* MDC CSPM features extended to your AWS resources. This agentless plan assesses your AWS resources according to AWS specific security recommendations and includes resutls in the secure score. The resources will also be assessed for compliance with built in standards specific to AWS (AWS CIS, AWS PCI DSS, and AWS Foundational security best Practices). MDC asset inventory page is a multicloud enabled feature helping you manage your AWS resources alongside your Azure resources.
* Microsoft Defender for containers extends its container threat detection and advanced defenses to your Amazon EKS Linux clusters.
* Microsoft Defender for Servers brings threat detection and advanced defenses to your Windows and Linux EC2 instances.

## Assess, secure and defend

MDC fills 3 vital needs as you manage security of resources and workloads in the cloud or on prem:

* Continuous assessment: Know your security posture, identify and track vulnerabilities.
* Secure: Harden resources and services with Azure security benchmark.
* Defend: Detect and resolve threats to resources, workloads and services.

## Continuosly assess

* Continuous Environment Assessment:
  * MDC provides continuous scanning and assessment of your cloud environment.
  * This is part of the Cloud Security Posture Management (CSPM) function, ensuring ongoing proactive security.
* Integrated Scanning Solutions:
  * MDC includes native vulnerability assessment solutions for key cloud assets.
  * Scanning targets include:
    * Virtual Machines (VMs)
    * Container Registries (where your container images are stored)
    * SQL Servers (your cloud-based databases)
* Enhanced Server Protection (Defender for Servers):
  * Automatic, Native Integration: When using Microsoft Defender for Servers, you get automatic, built-in integration with Microsoft Defender for Endpoint (MDE).
  * Leverages MDE Intelligence: This integration provides access to vulnerability findings from Microsoft threat and Vulnerability management (TVM) a highly detailed intelligence source.
* Comprehensive Coverage:
  * The combined assessment tools provide regular, detailed scans that cover multiple layers of the Defense-in-Depth model:
    * Compute (VMs, Servers, Containers)
    * Data (SQL Servers)
    * Infrastructure (the underlying cloud components)
* Centralized Reporting and Response:
  * All vulnerability findings and results from these detailed scans are aggregated and available within the MDC portal.
  * This provides a single place to review, prioritize, and respond to all discovered security weaknesses.

## Secure

* Goal: Ensure your cloud workloads are secure using core principles like Zero Trust and proper access control.

* Policy Engine: MDC uses Azure Policy for creating flexible security rules.
  * These rules can cover your entire environment (tenant, groups, subscriptions).

* Scalability & Monitoring:
  * It constantly checks new resources as you scale up.
  * It flags resources that are misconfigured.

* Guidance:
  * Recommendations are based on the Azure Security Benchmark (Microsoft's standard for security best practices).

* Prioritization:
  * Recommendations are grouped into Security Controls.
  * Each control contributes to your Secure Score, which is the single metric that tells you how healthy your security posture is.

## Defend

MDC also helps you defend your environment by providing security alerts and advanced threat protection features.

## Security alerts

When MDC detects a threat in any are of the environment it generates security alert. Security alerts: 

* Describe details of the affected resources.
* Suggest remediation steps.
* Provide in some cases an option to trigger a logic app in response.

MDC threat protection includes fusion kill chain analysis, which means that it automatically correlates alerts in your environment based on cyber kill chain analysis, to help you better understand the full story of an attack campaign where it started and what impact it had on your resources.

## Advanced threat protection

It provides advanced threat protection features for many of your deployed resources, including VMs, SQL databases, containers, web apps and your network. It includes securing the management ports of your VMs with just in time access, and adaptive app controls to create allowlistst for what apps should and shouldnt run on machines.
