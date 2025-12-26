# Describe the Microsoft Cost Management tool

## Table of contents
* [Cost management](#cost-management)
* [Cost alerts](#cost-alerts)
* [Cost management](#cost-management)
* [Cost alerts](#cost-alerts)
* [Budget alerts](#budget-alerts)
* [Credit alerts](#credit-alerts)
* [Department spending quota alerts](#department-spending-quota-alerts)
* [Budget](#budget)

## My interpretations

## Cost management

### Why does it exist?

It exists primarily to address the challenges of managing and controlling expenses in a flexible, usage based cloud environment. In simple terms it performs 3 main functions: Analyze, monitor and optimize.

### What does it do?

Its an essential suite of functions that helps you understand, control and optimize spending across Azure and other Microsoft cloud services.

### How does it work?

It works by ingesting raw usage data, applying rates and context (like tags) to calculate costs and then displaying this information for analysis, budget monitoring and optimization action.

### Summary

Cost management tool helps you control, manage your expenses. It uses raw data which it monitors and then analyses and optimizes accordingly to cut expenses or provide recommendations which resources are more costly, where to cut, what alternatives are etc. It adds tags to your data to calculate costs which it then displays the information for analysis which in return helps budget monitoring and optimizing actions. 

Basically you use it to explore and analyze your organizational costs. You can view aggregated costs by organization to understand spending habits and identify trends, you can filter accumulated costs over time to estimate monthly, quarterly or even yearly cost trends against a budget.

## Cost alerts

Cost alerts provide a single location for a quick check on all different alert types that may show up in the cost management service.

The 3 types that may show up:

* Budget alerts
* Credit alerts
* Department spending quota alerts

## Budget alerts

### Why does it exist?

It exists as a proactive financialy safety guardrail to send immediate warnings when spending is approaching or forecasted to exceed a defined limit, preventing unexpected overruns and enabling quick intervention to cut or adjust costly resources.

### What does it do?

It monitors your cloud spending against a set limit and sends you an early warning notification so you can take action before you overspend i.e cut resouces or services you no longer use etc.

### How does it work?

You define a budget for a specific scope (Subscription, resource group or tags). Azure continously tracks your actual cost and calculates a forecasted cost based on usage trends. Treshold checking compares these costs against the alert treshold you set and when a treshold is met it sends notification or triggers automated response to cut off or scale down resources.

### Summary

Budget alerts basically warn you if your set budget is crossing a treshold, you dont want to overspend your given budget so when that happens you get an alert to basically either cut off services/ resources or scale down resources.

## Credit alerts

### Why does it exist?

It exists to prevent unexpected costs by warning organizations when they are about to fully consume their pre paid Azure monetary commitment (credit).

### What does it do?

It notifies account owners when their Enterprise Agreement (EA) Azure credit balance reaches specific consumption thresholds (90% and 100%).

### How does it work?

It automatically tracks the EA (Enterprise agreements) credit balance and generates an alert that is sent via email and reflected in cost alerts when the 90% and 100% consumption thresholds are met.

### Summary

Credit alerts are an automatic financial warning system for EA customers that notifies account owners when their pre paid monetary commitment is nearly or fully consumed to avoid moving to standard billing rates.

## Department spending quota alerts

### Why does it exist?

To ensure department spending remains within predetermined limits set in the EA portal preventing departmental overruns.

### What does it do?

It notifies department owners when their departments cumulative spending hits a configured fixed quota threshold (e.g. 50% or 75%).

### How does it work?

It monitors department spending against a quota configured in the EA portal and when a defined threshold is reached it sends an email to owners and generates a cost alert.

### Summary

Department spending quota alerts are an EA mechanism that automatically monitors department expenditure against a fixed budget and notifies owners when consumption hits specific threshold percentages.

## Budget

### Why does it exist?

To enforce a spending limit (budget) on specific Azure scope (subscription or resource group) and warn users before they exceed that limit.

### What does it do?

It triggers an alert that appears in the cost alerts area and sends an email notification when Azure spending hits a pre set financial threshold.

### How does it work?

You define a spending limit for a scope and when spending reaches the set alert level a notification is generated. Advanced use allows this trigger to automate resource modification or suspension.

### Summary

Azure budgets are a spending limit you set. They send a warning when you get close to it and can even be set up to turn off services automatically to prevent unexpected charges.
