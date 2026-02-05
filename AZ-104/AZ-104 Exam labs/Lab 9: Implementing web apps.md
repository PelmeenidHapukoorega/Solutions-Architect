# Lab 9: Implementing web apps

### Goal:

Hosting web apps for company websites because because current ones are hosted on prem and nearing end of life, therefore in need to be replaced. To avoid new hardware costs Azure will be used to host new ones.

**Job Skills**

* Creating and configuring web apps.
* Creating and configuring deployment slots.
* Configuring web app deployment settings.
* Swapping deployment slots.
* Configuring and testing autoscaling for azure web apps.

## Part 1: Creating and configuring Azure web apps

1. In the portal went to `app services` > `+ create` > from drop down picked `web app`
2. On the `basics` tab selected new resource group where the web app would go in, named it `az104-rg9` added a name to the web app `SunStore` then filled out values:

* Publish > Code
* Runtime stack > PHP 8.2
* Operating system > Linux
* Region > Norway East
* Pricing plans > Premium V3 P1V3

For zone redundancy accepted defaults.

<img width="687" height="737" alt="image" src="https://github.com/user-attachments/assets/84641ea3-fd96-4e08-857d-1c2054ac4c5c" />

Hit create.

## Part 2: Created and configured deployment slot

### Goal:

Needed to create staging deployment slot which would enable me to perform testing before making the app available to the public. After i have performed testing i then could swap the slot from development or staging to production. This is a good practice to confirm that the web app is configured and works the way it is intended before public use.

1. Went to the `SunStore` web app resource and noticed the default domain link: `sunstore-djagfredd3exfkhv.norwayeast-01.azurewebsites.net` this is the domain of the web app itself.
2. Went to `deployment` > `deployment slots` in the web app and then > `add slot`.

Named it `staging` and made sure settings wouldnt be cloned:

<img width="571" height="234" alt="image" src="https://github.com/user-attachments/assets/2c1eb5ac-08d1-4c46-b33c-f874e54cb7e5" />

Hit add.

Noticed that both were running:

<img width="540" height="138" alt="image" src="https://github.com/user-attachments/assets/9143082e-8c8c-4dd5-a6f5-4717c96190be" />

When i selected the staging slot i noticed the URL was different. This is great because it meant that if the staging slot had its own URL i could actually see and interact with the changes prior to moving to production. Ease of comparison if you will.

## Part 3: Configuring web app deployment settings

1. In the `staging` slot went to > `deployment` > `deployment center` > settings.
2. Then selected from the drop down `external git` but noticed other options too such as github, which told me that i could connect my github for ease of automation to make changes to the web app.
3. In the `repository` field added `https://github.com/Azure-Samples/php-docs-hello-world`
4. In the branch field added `master` then hit save.

<img width="618" height="495" alt="image" src="https://github.com/user-attachments/assets/567690fa-8798-4406-85fd-0401bfce0937" />

Went back to overview page, took domain link and verified it now displayed `hello world` message:

<img width="782" height="127" alt="image" src="https://github.com/user-attachments/assets/3187dee0-c6ed-4ddc-bc6d-e9da462740ac" />

## Part 4: Swapped deployment slots

Here i learned how to swap from staging slot to production slot aka not ready to ready logic.

1. Went back to `deployment slots` > then selected `swap` from the top menu.

Since i had only 2 environments and 1 was already marked as production slot then it automatically filled the blanks for me:

<img width="572" height="452" alt="image" src="https://github.com/user-attachments/assets/009aa25f-b645-4b8c-ae63-d7019bcb1669" />

Started the swap.

2. Then i went back to `app services` chose my app service > on the overview page i copied the domain and pasted it to browser. Since i performed a swap i now expected to see `hello world` for the original domain:

<img width="794" height="128" alt="image" src="https://github.com/user-attachments/assets/c5cbdf3e-1dc4-4165-abf7-4bd9bbc118bc" />

Success.

## Part 5: Configuring and testing autoscaling of the web app

1. In the app service went to > `app service plan` then > `scale out` > used `automatic` option under scaling section, but noticed `rule based` option too which lets me determine scaling based on app metrics if i wanted to.
2. In the `maximum burst` field selected 2 and hit save.

Then i was met with an error `The parameter ‘MinimumElasticInstanceCount’ has an invalid value.
The desired MinimumElasticInstanceCount (0) for the site must be greater than zero.`

After troubleshooting i realised that Azure thought that minimum elastic instance count was 0 even though my UI showed `always ready=1`

I then switched from automatic to manual, hit save. Which then reset the hidden elastic minimum to match the visible settings. Switched it back to automatic, kept always ready 1 and added maximum burst 2, hit save and this time no problems.

Here is saved settings:

<img width="670" height="785" alt="image" src="https://github.com/user-attachments/assets/84c2e81e-7a17-4ec1-ac84-67cf87a08931" />

3. Went to `diagnose and solve problems` on the left panel.
4. Scrolled down and selected `Load test your app` > `create load test` > `+ create` and named it `pressure`:

<img width="804" height="515" alt="image" src="https://github.com/user-attachments/assets/9515aa4d-7073-4655-a67c-b58944e84247" />

Hit create.

5. Went to `pressure` resource, then in the overview > selected `create by adding http requests` > `create`
6. In the `Test plan` tab added new request and in the URL field pasted my default domain URL which i had already open in subsequent browser tab.

<img width="847" height="510" alt="image" src="https://github.com/user-attachments/assets/edc4dbf8-f136-4d9c-9541-e7417b9a17fc" />

Added that and hit review + create:

<img width="627" height="796" alt="image" src="https://github.com/user-attachments/assets/271ec28e-5651-40d2-8b1c-d59cc6afe933" />

Reviewed settings then hit create.

Then the test ran began:

Client side:

<img width="946" height="607" alt="image" src="https://github.com/user-attachments/assets/6b140185-59a2-492c-baac-767e463c3e90" />

From this i could gather:

* Virtual users were ramped to 50, so the test reached its planned load. No sign of throttling or cline side stalls.
* P90 response time 103ms, meaning it was fast and under load the app stayed responsive, no spikes no degradation.
* Requests/sec 457 avg peaking 600 meaning the app handled a high request rate consistently and throughput stayed stable even at max VUs.
* Errors 0, no HTTP failures, timeouts or client side exceptions.

Server side metrics:

<img width="930" height="592" alt="image" src="https://github.com/user-attachments/assets/f1faf98d-8d10-4aab-94c0-8daab737605f" />

From this i could gather:

* CPU 57% avg peaking around 60%, healthy nowhere near saturation and plenty of headroom.
* Memory 11%, extremely low, no pressure at all.
* Network throughput 3.67MB/s avg, consistent with the request volume and no signs of throttling or NIC saturation.

Overall the test was a success.

I then hit `stop` at the top to complete the test run then performed cleanup.

### Summary

Started by creating new azure web app to replace aging on prem websites. Set up the `SunStore` app using PHP on linux with  Premium plan in Norway East region. This gave me a clean, modern hosting environment without touching physical hardware.

Created staging deployment slot which immediately showed why slots matter: each slot gets its own URL. This meant i could test changes safely before exposing anything to production.

Configured deployment for the staging slot using an external Git repository. After saving the settings the staging URL showed the “hello world” sample app, confirming the deployment pipeline worked.

Performed slot swap moving staging → production. After the swap the main domain showed the updated content. This demonstrated how Azure handles zero downtime transitions between environments.

Set up autoscaling on the App Service Plan. Ran into the hidden “MinimumElasticInstanceCount” issue and fixed it by toggling scaling modes and then applied the correct settings: Always Ready = 1, Maximum Burst = 2.

Created and executed load test to see how the app behaved under pressure. The client side metrics showed fast responses, no errors, and stable throughput. The server side metrics showed CPU around 60%, low memory usage, and steady network throughput. The app handled the load without any signs of stress.

Then i stopped the test and cleaned up the resources afterward.

### What i learned

* Learned how easy it is to deploy and manage web apps in Azure compared to traditional servers.
* How deployment slots give you a safe workflow for testing and swapping changes.
* Learned how Git‑based deployments streamline updates.
* How autoscaling works and how to troubleshoot the quirks behind the scenes.
* How to run realistic load tests and interpret both client‑side and server‑side performance metrics.

### Key Takeaways

* Azure App Services lets you quickly build, deploy, and scale web apps.
* App Service includes support for many developer environments including ASP.NET, Java, PHP, and Python.
* Deployment slots allow you to create separate environments for deploying and testing your web app.
* You can manually or automatically scale a web app to handle additional demand.
* A wide variety of diagnostics and testing tools are available.
