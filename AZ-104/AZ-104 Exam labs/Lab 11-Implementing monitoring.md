# Lab 11: Implementing monitoring

### Goal:

Learn how to backup and recover VMs. Learning how to create recovery service vault and backup policy for VMs as well as learning about disaster recovery with site recovery.

```ascii
_________________________________________________________________________
| [ az104-rg11 ]                                                        |
|                                                                       |
|  +-------------------+        +------------------------------------+  |
|  |      TASK 1       |        |               TASK 2               |  |
|  |   az104-11-vm0    |---┐    |   Alert - delete virtual machine   |  |
|  +-------------------+   |    +------------------------------------+  |
|                          |                                            |
|                          |    +------------------------------------+  |
|                          |    |               TASK 3               |  |
|                          |    |        Action - send email         |  |
|                          |    +------------------------------------+  |
|                          |                                            |
|  +-------------------+   |    +------------------------------------+  |
|  |      TASK 6       |   |    |               TASK 4               |  |
|  |    Log Queries    |   └--->|         Trigger the alert          |  |
|  +-------------------+        +------------------------------------+  |
|                                                                       |
|                               +------------------------------------+  |
|                               |               TASK 5               |  |
|                               |       Add a processing rule        |  |
|                               +------------------------------------+  |
|_______________________________________________________________________|
 ```

**Job Skills**

* Using template to provision infra
* Creating alerts
* Configuring action group notifications
* Triggering an alert and confirming that it functions
* Configuring alert processing rule
* Using monitor log queries

1. Created new resource group `az104-rg11`. Then in `deploy a custome template` uploaded the template.json file i had downloaded from the official repo, then selected my resource group and for region took norway east.

<img width="440" height="522" alt="image" src="https://github.com/user-attachments/assets/8975e80b-f01a-4a1a-a7b4-023bb28eb856" />

2. Went to `Monitor` > `view` in Vm insights box then > `configure insights`.
3. Enabled the setting next to VM and went for review:

<img width="697" height="393" alt="image" src="https://github.com/user-attachments/assets/6ce70ff2-2245-41ba-905a-615b5788f347" />

## Part 2: Creating an alert

1. Continued in the `monitoring` and went to create new alert rule under `alerts`
2. Selected my subscription to apply the rule on subscription level.
3. Then in the `condition` tab selected `see all signals` and searched for then selected `delete virtual machine`, this would apply alert that whenever VM would be deleted, alert would be sent out.
4. In `alert logic` reviewed `event level` sections and left default of `all selected`:

<img width="927" height="754" alt="image" src="https://github.com/user-attachments/assets/65dcaa03-d6be-4439-9113-70a8aec1128d" />

## Part 3: Configuring action group notifications

1. In the same pane went to `actions` > `use action groups` and > `create action group` in select action group blade.
2. Filled out the basics tab:

<img width="939" height="491" alt="image" src="https://github.com/user-attachments/assets/868c7083-60c2-43ff-b21c-02a2700e43d2" />

3. Moved to `notifications`, selected they type on how the alert would be recieved > `Email/SMS message/push/voice` and named it `VM was deleted`.
4. Added my email and additionally my phone number then went for a review:

<img width="780" height="467" alt="image" src="https://github.com/user-attachments/assets/5a193420-ddf5-42f0-ba42-dce877da839e" />

Hit create.

**Note**

When being added to an action group you will recieve email notification about it, however it may come with a delay.

5. In the create an alert rule page went to `details` and added alert rule name `VM was deleted` and description `VM in resource group was deleted`, although in hindsight that means you could create rule, apply it to resource group and add a description that would specifically mention in what resource group and what specific VM was deleted for ease of management when having a lot of resources.

Went for a review:

<img width="910" height="660" alt="image" src="https://github.com/user-attachments/assets/3fe0fec7-5c77-4fea-8ce2-eff9e8b95cdf" />

Hit create.

## Part 4: Triggerin an alert and confirming it works

1. Went to my VM and deleted it with `apply force delete` checkbox then hit delete.
2. Waited for email alert and SMS:

Email:

<img width="724" height="758" alt="image" src="https://github.com/user-attachments/assets/9618c462-72ff-4b42-bbfb-aeb0ef7941eb" />

SMS:

<img width="922" height="2048" alt="image" src="https://github.com/user-attachments/assets/52b78725-ca2f-4091-8c20-3fa3dfc70422" />

## Part 5: Configuring lert processing rule

The goal here was to create alert rule that would supress notifications during a maintenance period.

1. Continued in `monitor` > `Alerts` and went to > `Alert processing rules` to create a new one.
2. Applied it at subscription level, applied it.
3. In `Rule settins` selected `supress notifications` then went to `scheduling`

By default the rule works all the time unless i disable it or configure a schedule when to trigger.

Chose to apply the rule `at a specific time`, selected start date and end date as well as local timezone.

4. Went to `details` tab, selected my resource group, named the rule and added description.

Review:

<img width="907" height="559" alt="image" src="https://github.com/user-attachments/assets/49b5aff7-5610-4e40-a103-6d5393818040" />

Hit create.

## Part 6: Using Azure monitor log queries

1. In the `monitor` opened up `Logs`
2. Selected my scope (subscription) and applied it.
3. In the `queries` tab selected `virtual machines` then from drop down looked for `count heartbeats` and clicked `run` next to it:

<img width="447" height="250" alt="image" src="https://github.com/user-attachments/assets/2f58e992-1ddb-4bd8-a61f-2e48dd8ce287" />

4. Switched modes from `simple mode` to `KQL mode` in the top right. The query would now use heartbeat table.
5. Replaced the query with the following:
```
 InsightsMetrics
 | where TimeGenerated > ago(1h)
 | where Name == "UtilizationPercentage"
 | summarize avg(Val) by bin(TimeGenerated, 5m), Computer //split up by computer
 | render timechart
```

And ran it:

<img width="562" height="502" alt="image" src="https://github.com/user-attachments/assets/a342c8ae-429f-4e98-a580-c4c98c7f7b19" />

This showed me when the VM was provisioned and when exactly it flatlined.

Performed cleanup by deleting the workspace first. By deleting workspace azure will then remove deny assignments, managed resource group, DCRs, DCEs and AMA internal resources.

### Summary

I started by deploying the lab environment using the provided ARM template. Loaded the template into the custom deployment blade, selected my resource group, and deployed the VM and supporting resources automatically.

Then i enabled VM Insights by going to Azure Monitor and configuring insights for the VM. This allowed Azure to start collecting performance and health data for monitoring.

Created an alert rule that triggers whenever a virtual machine is deleted. Selected the “Delete Virtual Machine” signal, reviewed the alert logic, and kept the default event levels. Configured an action group to define how notifications should be delivered. I Added email and SMS notifications, named the action, and completed the setup. I then attached this action group to the alert rule and finalised the alert configuration.

Triggered the alert by deleting the VM with force delete enabled. Waited for the notifications and confirmed that both the email and SMS alerts were delivered successfully. Then i created an alert processing rule to suppress notifications during a maintenance window. Applied it at the subscription level, configured the schedule, and added a name and description before creating the rule.

Used Azure Monitor Logs to run queries. Started with the built in “count heartbeats” query, then switched to KQL mode and ran a custom query against the InsightsMetrics table to visualise CPU utilisation over time. This showed exactly when the VM was provisioned and when it stopped sending metrics.

Completed the lab by cleaning up the workspace, which automatically removed associated monitoring resources.

### What i learned

* Learned how to deploy monitoring‑ready infrastructure using ARM templates.
* Learned how to enable VM Insights to collect performance and health data.
* Learned how to create alert rules based on platform events such as VM deletion.
* Learned how action groups define who gets notified and how.
* Learned how to test alerts by triggering real events and verifying notifications.
* Learned how alert processing rules help suppress noise during maintenance windows.
* Learned how to run KQL queries in Azure Monitor Logs to analyse VM behaviour.

### Key Takeaways

* Alerts help you detect and address issues before users notice there might be a problem with your infrastructure or application.
* You can alert on any metric or log data source in the Azure Monitor data platform.
* An alert rule monitors your data and captures a signal that indicates something is happening on the specified resource.
* An alert is triggered if the conditions of the alert rule are met. Several actions (email, SMS, push, voice) can be triggered.
* Action groups include individuals that should be notified of an alert.
