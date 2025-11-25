# Microsoft Cloud Adoption Framework (Best practices)

[![Azure](https://img.shields.io/badge/azure-%230072C6.svg?style=for-the-badge&logo=microsoftazure&logoColor=white)](https://learn.microsoft.com/et-ee/training/modules/cloud-adoption-framework/)

[![Repo Status](https://img.shields.io/badge/Status-Active-success?style=for-the-badge&logo=github)](https://github.com/PelmeenidHapukoorega/Solutions-Architect)

[![Microsoft Learn Plan](https://img.shields.io/badge/Study_Plan-Microsoft_Learn-0078D4?style=for-the-badge&logo=microsoft)](https://learn.microsoft.com/et-ee/plans/d8gdbny2gjwr62)

If you wanna actually adopt the cloud the right way you gotta stop thinking of Azure like some big magic button and start seeing it like a whole roadmap. Strategy plan ready migrate innovate govern manage secure every step snaps together like a blueprint that tells you what to build why to build it and how not to blow your whole environment up by guessing.

Cloud aint just servers in the sky its a full lifestyle change for your tech your teams and your decisions. The Cloud Adoption Framework is basically the cheat code that keeps you from drifting around blind and crashing your budget like a rookie.

**Bottom line:**
Follow the framework and you move smooth. Ignore it and you gonna end up troubleshooting your own bad decisions like you speedran cloud chaos percent any% no safety gear.

## Table of Contents

* [Strategy](#strategy)
* [Plan](#plan)
* [Ready](#ready)
* [Migrate](#migrate)
* [Modernize](#modernize)
* [Cloud-native](#cloud-native)
* [Govern](#govern)
* [Manage](#manage)
* [Secure](#secure)

## Strategy

**Key points**
* The Strategy methodology aligns cloud adoption with business goals using five specific steps.
* It ensures you aren't just moving servers but are actually solving business problems (Cost, Agility, Scalability).

**The 5 Steps of Strategy**

1.  **Assess your strategy**
    *   Evaluate your current maturity and identify gaps.
    *   **Tool:** Use the **Cloud Adoption Strategy Evaluator** to get a baseline posture.

2.  **Determine motivations, mission, and objectives**
    *   **Motivations:** Identify *why* you are moving (Cost savings, Innovation, Agility).
    *   **Critical Events:** If a business event (e.g., datacenter lease expiry) is a priority, migration might start early alongside planning.
    *   **KPIs:** Define specific Key Performance Indicators to measure if your objectives are actually successful.
    *   **Accountability:** Assign ownership for every key result.

3.  **Define your team**
    *   **Goal:** Facilitate alignment between business and tech.
    *   **Core Team:** Should include IT, Finance, and Security.
    *   **Key Role:** Lead Architect translates business objectives into technical architectures.
    *   **Expansion:** Start small with the core team, then involve HR/Marketing as you expand.

4.  **Prepare your organization**
    *   **Alignment:** Ensure Business, Digital, and IT strategies all support the same goal.
    *   **Operating Model Shift:**
        *   *From:* **Project Delivery** (Task-driven, defined scopes, timelines).
        *   *To:* **Product Delivery** (Continuous, outcome-driven, cross-functional teams).

5.  **Inform your strategy**
    *   **Financial Efficiency:** Minimize unnecessary expenses.
    *   **AI:** Integrate analytics and automation for growth.
    *   **Resiliency:** Design infrastructure to survive disruptions.
    *   **Security:** Adopt a **Zero Trust** strategy early.
    *   **Sustainability:** Incorporate green practices into the design.

**Takeaway**
Strategy is not just a document; it is the shift from "Project Delivery" to "Product Delivery" and ensuring your Finance, Security, and IT teams are actually building the same thing.

## Plan

**Key points**
* Cloud adoption requires more than just readiness; it needs a plan that converts strategy into actionable steps.
* The planning phase aligns your organization, people, and existing digital estate with your cloud goals.

**Steps to Plan**

1.  **Prepare your organization**
    *   **Startups:** Focus on "Cloud-native" development (Plan -> Ready -> Cloud-Native).
    *   **Enterprises:** Must evaluate the full IT estate (Plan -> Ready -> Migrate -> Modernize).
    *   **Management Models:**
        *   *Small:* Centralized operations (consistent policy).
        *   *Mid-Size:* Shared management (Platform team manages Landing Zones, Workload teams manage apps).
        *   *Skilled/Large:* Decentralized operations (Full ownership by workload teams).
    *   **Document Responsibilities:** Clearly map who owns Governance, Security, and Operations.

2.  **Prepare your people**
    *   **Skills Gap:** Assess if teams know Governance, Identity, and Networking.
    *   **RAMP Model:** Azure environment managers need skills to **R**eady, **A**dminister, **M**onitor, and **P**rotect.
    *   **Sustain Skills:** Use learning sandboxes (Azure Dev/Test subscriptions) and dedicate weekly time for Microsoft Learn.

3.  **Discover existing workload inventory**
    *   **Inventory:** Use automated tools like **Azure Migrate** to find everything (servers, apps, dependencies).
    *   **Prioritize:** Create a migration backlog based on **Business Criticality** vs. **Technical Feasibility**.
    *   **Gather Details:** Document ownership, compliance needs, and data sensitivity for every workload.

4.  **Select migration strategies (The 5 R's + 2)**
    *   **Retire:** Turn off redundant workloads.
    *   **Rehost:** Lift and shift (Minimal disruption).
    *   **Replatform:** Move to PaaS (e.g., SQL MI) without changing code significantly.
    *   **Refactor:** Optimize code for cloud.
    *   **Rearchitect:** Rebuild for cloud-native capabilities.
    *   **Replace:** Switch to SaaS (e.g., Exchange to Microsoft 365).
    *   **Retain:** Keep it as-is (on-prem) if it is stable and not worth moving.

5.  **Assess workloads for migration**
    *   **Architecture:** Map dependencies (what talks to what).
    *   **Code:** Use **AppCAT** (Application Cloud Assessment Tool) for .NET/Java apps to check compatibility.
    *   **Databases:** Decide whether to use shared instances or split databases by workload.
    *   **Risk Register:** maintain a document tracking technical and operational risks.

6.  **Estimate Total Cost of Ownership (TCO)**
    *   **Architecture First:** You cannot estimate cost if you don't know what you are building.
    *   **Tools:** Use the **Azure Pricing Calculator** based on historical usage (not just guess-work).
    *   **Operational Costs:** Include the cost of training staff and changing processes, not just the Azure bill.

**Takeaway**
Planning tells you exactly *what* you have (Inventory), *who* will manage it (Skills/Roles), and *how much* it will cost (TCO) before you spend a dime.

## Ready

**Key points**
* The Ready methodology guides the setup of your Azure environment, operating model, and landing zones.
* It ensures you have a "foundation" before you start deploying solutions, focusing on organizing resources, controlling costs, and securing the environment.

**Steps to Ready**

1.  **Define a cloud operating model**
    *   **Concept:** Shifts focus from physical hardware to digital assets and workloads.
    *   **Goal:** Ensure consistent operations across the environment.
    *   **Components:** Alignment to business strategy, organization of people, change management, operations management, governance, and security.
    *   **Action:** Compare common operating models to find the one that fits your specific needs.

2.  **Implement Landing Zones**
    *   **Definition:** A scalable, modular environment that provides the foundation for security, governance, and resource management.
    *   **Tools:** Deploy using Azure Portal, Bicep, or Terraform.
    *   **Continuous Optimization:**
        *   Eliminate unnecessary expenses.
        *   Enhance application performance.
        *   Mitigate security vulnerabilities.
        *   Ensure efficient scaling.
        *   Maintain compliance with regulations.

3.  **Develop necessary skills**
    *   Align teams with cloud adoption functions (may require new org structures).
    *   Use Microsoft learning paths to close technical gaps.
    *   Assign specific roles for the new operating model.

4.  **Avoid Antipatterns**
    *   **Inadequate preparation:** Rushing into deployment without a landing zone.
    *   **Misconception about features:** Assuming cloud services work exactly like on-prem hardware.
    *   **Lack of knowledge:** Not understanding how the cloud provider operates vs. what you manage.

**Takeaway**
"Ready" is about building the digital datacenter (Landing Zone) and defining the rules of the road (Operating Model) so you don't crash when you start driving fast.

## Migrate

**Key points**
* Migration involves planning, executing, and optimizing workloads moving from on-prem or other clouds to Azure.
* The goal is to minimize risks, reduce costs, and ensure successful adoption.

**Steps to Migrate**

1.  **Plan migration**
    *   **Assess Skills:** Evaluate Azure capabilities (Infra, Security, Apps) and fill gaps with partners.
    *   **Data Path:**
        *   *ExpressRoute:* High bandwidth.
        *   *VPN:* Encrypted connections.
        *   *Data Box:* Offline bulk migration.
    *   **Sequence:** Map dependencies using **Azure Migrate** and prioritize by business criticality.
    *   **Method:** Choose **Near-zero downtime** (Mission critical) vs. **Planned downtime** (Maintenance windows).
    *   **Rollback:** Always have automated recovery scripts and tested rollback procedures.

2.  **Prepare workloads for the cloud**
    *   **Compatibility:** Replace hardcoded configs with **Azure Key Vault** and eliminate local dependencies.
    *   **Validation:** Test connectivity, auth flows, and performance (Azure Load Testing).
    *   **Infrastructure as Code:** Create **ARM templates** or **Bicep** files for reusable deployment.
    *   **Documentation:** Create operational runbooks and troubleshooting guides.

3.  **Execute migrations**
    *   **Change Freeze:** Stop unauthorized changes on source systems during the window.
    *   **Finalize Prod:** Deploy infra via tested templates and apply security policies.
    *   **Cutover:**
        *   *Near-zero:* Configure replication, migrate static files, pause writes, redirect traffic.
        *   *Planned:* Stop operations, move data, test, redirect.
    *   **Fallback:** Retain source infrastructure until validation is complete.
    *   **Stabilization:** dedicated support teams and enhanced monitoring immediately after move.

4.  **Optimize workloads after migration**
    *   **Fine-tune:** Apply **Azure Advisor** recommendations and security fixes.
    *   **Feedback:** Collect user feedback to identify issues early.
    *   **Reviews:** Conduct quarterly reviews using **Well-Architected Framework** tools.
    *   **Share Outcomes:** Track cost savings via **Azure Cost Management** and report operational benefits to stakeholders.

5.  **Decommission source workloads**
    *   **Approval:** Get written sign-off from business owners before turning things off.
    *   **Licenses:** Identify licenses eligible for **Azure Hybrid Benefit** to save money.
    *   **Data Retention:** Move compliance data to **Azure Blob Storage** for long-term retention before wiping disks.
    *   **Update:** Archive legacy documentation with deprecation notices.

**Takeaway**
Migration is not just "moving files." It is a disciplined process of Planning (Sequence/Rollback), Execution (Change Freeze/Cutover), and Decommissioning (Reclaiming Licenses) to ensure you actually shut down the old cost center.

## Modernize

**Key points**
* Cloud modernization improves existing workloads to meet business needs without adding new features.
* It focuses on aligning with cloud best practices (Well-Architected Framework) rather than just lifting and shifting.

**Steps to Modernize**

1.  **Prepare organization**
    *   **Definition:** Modernization = Replatform, Refactor, or Rearchitect. It is *not* a complete rewrite.
    *   **Prioritize:** Create a matrix of **Business Value** (Revenue, Customer Experience) vs. **Technical Risk** (Tech debt, Scalability limits).
    *   **Assess:** Use the **Azure Well-Architected Framework (WAF)** to identify gaps across Reliability, Security, Cost, Operations, and Performance.

2.  **Plan your cloud modernization**
    *   **Choose Strategy:**
        *   *Replatform:* IaaS to PaaS (Quick win, minimal code).
        *   *Refactor:* Modify code for cloud optimization.
        *   *Rearchitect:* Cloud-native redesign (Microservices/Serverless).
    *   **Deployment Strategy:**
        *   *In-place:* Maintenance windows (Low risk).
        *   *Parallel:* Blue/Green or Canary releases (High risk/High uptime).
    *   **Risk Mitigation:** Automate rollbacks and detailed rollback procedures.
    *   **Stakeholders:** Quantify value (e.g., "20-40% cost reduction").

3.  **Execute modernizations**
    *   **Non-Production:** Develop in an environment that mirrors production (using IaC).
    *   **Validation:**
        *   Test at **150%** of expected load.
        *   Security validation via **Microsoft Defender for Cloud**.
    *   **Deployment:**
        *   *Parallel:* Use weighted routing (start at 1%, increase incrementally).
        *   *Cutover:* Keep the old environment as a **hot standby** for 24-72 hours post-switch.
    *   **Support:** Provide "Hypercare" (enhanced support SLAs) during stabilization.

4.  **Optimize workloads**
    *   **Config:** Apply **Azure Advisor** recommendations systematically.
    *   **Readiness:** Verify **Azure Monitor** logs and **Cost Management** budget alerts.
    *   **Continuous:** Automate optimization using **Azure Policy** and **Autoscaling** rules.

**Takeaway**
Modernization is the difference between "running in the cloud" and "running *for* the cloud." It turns a static, expensive VM architecture into a scalable, cheaper PaaS solution.

## Cloud-native

**Key points**
* Cloud-native solutions create new business value by building applications from scratch (or adding features) that specifically use cloud capabilities like scalability, resilience, and agility.
* Unlike "Migrate," this is about building *for* the cloud, not just moving *to* the cloud.

**Steps to Cloud-native**

1.  **Plan cloud-native solutions**
    *   **Objectives:** Define measurable business goals and constraints. Establish what is in-scope vs. out-of-scope.
    *   **Architecture:** Use validated patterns from the **Azure Architecture Center**. Integrate the **Well-Architected Framework** pillars into design decisions.
    *   **Deployment Strategy:** Plan for **DevOps** automation. Use **Progressive Exposure** (start with pilot groups) and patterns like **Blue-Green** deployment for major changes.
    *   **Rollback:** Create comprehensive procedures to recover quickly from deployment issues.

2.  **Build cloud-native solutions**
    *   **Development:** Build in non-production environments that mirror production.
    *   **Tooling:** Implement Source Control with **CI/CD pipelines**. Integrate **Azure Monitor** and **Application Insights** from the very start.
    *   **Infrastructure:** Create standardized, reusable infrastructure patterns to ensure configuration consistency.

3.  **Deploy cloud-native solutions**
    *   **Stakeholders:** Announce schedules and expected impacts before touching production.
    *   **Execute:** Use validated CI/CD pipelines. Perform **Smoke Tests** to verify core functionality.
    *   **Progressive Rollout:** Expose new systems to small user groups first, then expand based on monitoring.
    *   **Stabilization:** Keep developers on-call alongside ops during the first 1-2 weeks. Define clear exit criteria for stabilization.

4.  **Optimize after deployment**
    *   **Fine-tune:** Apply **Azure Advisor** recommendations weekly for cost and security.
    *   **Cost Control:** Set budgets and shut down non-production environments during off-hours.
    *   **Reliability:** Test **Azure Backup** and recovery procedures (actually perform trial restores).
    *   **Evolve:** Schedule periodic **Well-Architected Framework** reviews to assess architecture against changing usage patterns.

**Takeaway**
Cloud-native is about agility and automation. You don't just deploy a server; you build a pipeline that deploys a self-healing, auto-scaling application that is monitored from day one.

## Govern

**Key points**
* The Govern methodology provides a structured approach to maintain control and address risks in Azure.
* It consists of five steps to establish and optimize governance without slowing down innovation.

**The 5 Steps of Governance**

1.  **Build a team**
    *   **Composition:** Select a small, diverse team to facilitate quick decision-making.
    *   **Role:** Define their authority and scope.
    *   **Support:** Ensure the organization backs the team so they can actually enforce security policies.

2.  **Assess cloud risks**
    *   **Identify:** Use Azure tools to list assets and discover potential risks.
    *   **Analyze:** Assign a qualitative or quantitative value to each risk and prioritize by severity.
    *   **Impact:** Determine the cost of the risk (e.g., Downtime, Financial loss).
    *   **Review:** Risks change. Review them regularly or in response to major events.

3.  **Document policies**
    *   **Purpose:** Policies mitigate the risks identified in step 2.
    *   **Structure:** Establish clear requirements, standards, and goals for IT staff and automated systems.
    *   **Repository:** Store policies in a centralized location so everyone knows the rules.

4.  **Enforce policies**
    *   **Automation:** Use cloud governance tools (like Azure Policy) to automate compliance. Start small and expand.
    *   **Strategy:**
        *   Delegate responsibilities.
        *   Adopt an **inheritance model** for policies (Management Group -> Subscription).
        *   Apply **Tagging** and **Naming** conventions.
        *   Start with a **Monitor-first** approach (Audit before Deny).

5.  **Monitor cloud governance**
    *   **Compliance:** Use monitoring to find areas that lack compliance.
    *   **Alerting:** Configure alerts with clear thresholds to notify the right people when policies are violated.
    *   **Remediation:** Have a plan to fix violations quickly and update policies to stop them from happening again.

**Takeaway**
Governance is not a blockade; it is a set of guardrails. By documenting risks and automating policies (Tagging, RBAC, Naming), you ensure teams can move fast without accidentally exposing data or blowing the budget.

## Manage

**Key points**
* Cloud management is about establishing effective operations, clear responsibilities, and standardized processes across your Azure estate.
* It covers everything from daily admin tasks and monitoring to reliability and security incident response.

**Steps to Manage**

1.  **Ready your Azure cloud operations**
    *   **Responsibilities:** Distinguish between central responsibilities (platform-wide) and workload-specific responsibilities (apps).
    *   **Teams:** Form dedicated platform teams vs. specialized workload teams based on org size.
    *   **Procedures:** Create standardized runbooks for change management and disaster recovery. Store them centrally.
    *   **Daily Ops:** Establish 24/7 support (on-call or global). Automate repetitive tasks to reduce error.
    *   **Continuous Improvement:** Weekly reviews of metrics and incidents. Address technical debt regularly.

2.  **Administer your Azure cloud estate**
    *   **Control Changes:** Use formal ticketing, **Change Analysis**, and **Azure Policy** to prevent unauthorized drift.
    *   **Security:** Use **Microsoft Entra ID** (RBAC, MFA, Conditional Access) and enforce least privilege.
    *   **Compliance:** Map policies to standards like **ISO 27001** and **NIST SP 800-53**.
    *   **Data Governance:** Use **Microsoft Purview** to classify data and control residency.
    *   **Cost:** Monitor spending via **Microsoft Cost Management**.
    *   **Resources:** Limit portal deployments to non-production. Use **IaC** (Bicep/Terraform) and CI/CD for production.
    *   **OS Maintenance:** Automate updates via **Azure Update Management**.

3.  **Monitor your Azure cloud estate**
    *   **Strategy:** Use **Azure Monitor** as the central hub and **Azure Arc** for multi-environment collection.
    *   **Scope:** Monitor Service Health, Security, Compliance, Cost, and Performance.
    *   **Alerting:** Define thresholds with dynamic alerts. Route notifications via SMS/Email or ITSM tools.
    *   **Visualization:** Use **Azure Monitor Workbooks** for deep analysis and Portal Dashboards for overviews.

4.  **Protect your cloud estate**
    *   **Reliability:** Assign **SLAs** and recovery objectives based on business criticality.
    *   **Data Protection:** Configure backup/replication to meet **RTO** (Time) and **RPO** (Point) requirements.
    *   **Resilience:** Design self-healing apps and deploy redundant infrastructure (Availability Zones/Regions).
    *   **Security Ops:** Use **Microsoft Sentinel** for monitoring and incident response.
    *   **Business Continuity:** Detect failures within one minute and have tested recovery procedures ready.

**Takeaway**
Management is the unglamorous work that keeps the lights on. If you don't automate your updates, monitor your logs, and test your backups, you don't own your environment—it owns you.

## Secure

**Key points**
* Security is not a separate phase; it is integral to every part of the Cloud Adoption Framework.
* All recommendations adhere to **Zero Trust** principles: **Assume Compromise**, **Least Privilege**, and **Explicit Verification**.

**Steps to Secure**

1.  **Take advantage of security guidance**
    *   **CAF Secure Methodology:** Guidance for teams managing the *infrastructure/platform*.
    *   **Azure Well-Architected Framework (WAF):** Guidance for *workload owners* (DevOps/DevSecOps) on specific apps.
    *   **Microsoft Cloud Security Benchmark:** The baseline for best practices and optimal configurations.
    *   **Zero Trust Guidance:** Technical capabilities for modernizing security strategy.
    *   **Focus:** Enhance posture through incident preparation and response to minimize the "blast radius" of any breach.

2.  **Use the CIA Triad model**
    *   **Confidentiality:** Only authorized individuals access data (Encryption, Access Controls).
    *   **Integrity:** Data is accurate and complete (Protecting against tampering).
    *   **Availability:** Resources are accessible when needed (Preventing downtime).
    *   **Benefits:** Ensures data protection, business continuity, and customer trust.

3.  **Assign roles**
    *   **Map & Assess:** Map existing roles, check for gaps, and invest to fill them.
    *   **Shared Responsibility:** Document a model similar to **RACI** (Responsible, Accountable, Consulted, Informed) to define who makes decisions vs. who executes.
    *   **Collaboration:** Ensure technical teams understand how to work with security teams.

4.  **Continuous Improvement**
    *   Cyber threats evolve constantly.
    *   Use **Retrospectives** and monitoring to find areas for improvement.
    *   Provide ongoing training to keep teams up to date with sophisticated threats.

**Takeaway**
Security is a continuous cycle of modernization and preparation. By using the **CIA Triad** and **Zero Trust** principles, you ensure that even if a breach occurs (Assume Compromise), the damage is contained and operations continue.
