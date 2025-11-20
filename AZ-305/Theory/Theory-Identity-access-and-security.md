# Module: Azure identity, access and security
# Key notes

If you wanna actually secure anything in Azure you gotta treat identity like the front door to your whole kingdom. Entra ID, RBAC, Conditional Access, Zero Trust thats the real backbone. Permissions, policies, sign ins, verifications they all lock together like a vault puzzle deciding who gets in, what they touch, and how bad things go if you slip up.

**Bottom line:**
Lock your house now, because Future You aint tryna explain to your boss why some rogue service principal just joy rode through your subscription like in GTA free roam mode.

## Table of Contents
- [Azure directory services](#azure-directory-services)
- [Azure authentication methods](#azure-authentication-methods)
- [Azure external identities](#azure-external-identities)
- [Azure conditional access](#azure-conditional-access)
- [Azure role based access control](#azure-role-based-access-control)
- [Zero Trust model](#zero-trust-model)
- [Defense in depth](#defense-in-depth)
- [Microsoft Defender for Cloud](#microsoft-defender-for-cloud)
#

## Azure directory services

**Key points**
* Microsoft Entra ID is the cloud directory that handles your sign ins and access to Azure and other Microsoft apps
* Works together with on premises Active Directory if you got that running already
* Microsoft handles global availability you control the identities
* Gives you security features like suspicious sign in detection smart lockout MFA and more

**What Entra ID actually does**
* **Authentication**
  * Verifies identity lets users reset passwords supports MFA protects logins  
* **Single sign on**  
  * One identity across apps makes access clean and easy  
* **Application management**  
  * Handle cloud and on prem apps through Entra tools  
* **Device management**  
  * Register devices manage them with Intune and apply conditional access rules

**Who uses it**
* IT admins controlling access and security  
* Developers adding SSO and identity to their apps  
* Regular users managing their own creds  
* Anyone using Microsoft 365 Azure CRM basically all of em run through Entra ID

**Takeaway**
* Entra ID is the identity backbone for everything you do in Azure  
* If the account aint right nothing else works right

### Connecting on premises AD with Microsoft Entra ID

**Key points**
* Without syncing you would have two totally separate identity systems  
* With Microsoft Entra Connect you sync the whole identity story between on prem AD and cloud Entra ID  
* You get unified identity SSO MFA password resets and synced account changes

**How Entra Connect fits**
```ascii
On Prem AD  
   ↓ sync  
Entra ID  
   ↓  
Cloud apps and resources
```

**Takeaway**
* Entra Connect makes on premises identity and cloud identity act like one system

### Microsoft Entra Domain Services

**Key points**
* Managed domain service that gives you domain join group policy LDAP Kerberos NTLM
* No domain controllers needed Azure handles them
* Perfect for legacy apps that need old school auth but you still want them in the cloud

**Why it matters**
* Lets you lift old apps into Azure without breaking them
* Users can sign in with existing Entra accounts
* No patching domain controllers no maintaining AD DS in the cloud

### How Microsoft Entra Domain Services works

**Key process**
* You create a managed domain and set a domain name
* Azure deploys two managed domain controllers behind the scenes
* Everything is synced one way from Entra ID → Entra Domain Services
* Apps and VMs inside Azure use the managed domain for join and authentication

**Simple diagram**
```bash
Entra ID  
   ↓ one way sync  
Managed Domain Services  
   ↓  
VMs apps legacy workloads
```

**Important!**
* Changes inside the managed domain do not sync back to Entra ID
* If you have on prem AD the sync chain looks like this:
On Prem AD → Entra ID → Domain Services

**Full flow diagram**
```bash
On Prem AD  
   ↓ Entra Connect  
Microsoft Entra ID  
   ↓ one way sync  
Microsoft Entra Domain Services  
   ↓  
Apps VMs LDAP Kerberos NTLM
```

**Takeaway**
* Domain Services lets old school apps live in the cloud without you babysitting domain controllers

### Summary
**Microsoft Entra ID**
  * The cloud identity boss that handles authentication SSO app access device registration and security intelligence
**Entra Connect**
  * The sync bridge that merges your on prem AD with cloud identity so everything acts unified
**Entra Domain Services (EDS)**
  * The managed old school domain environment for legacy workloads that need domain join and classic auth without you           running DCs

## Azure authentication methods

**Key points**
* Authentication is proving who you are using some kind of credential  
* Azure supports passwords SSO MFA and full passwordless setups  
* Newer auth methods hit that sweet spot of high security and high convenience  
* Passwords alone are weakest even if they easy  

**Security vs convenience diagram**
```txt
Low security  High convenience      = passwords  
High security Low convenience       = passwords plus MFA  
High security High convenience      = passwordless  
Low security Low convenience        = nobody uses that  
```

**Takeaway**
* Auth is the front door of every cloud environment
* Choose the right lock or somebody else walks in for you

### Single sign on SSO
**What it is**
* You sign in once and use that identity across multiple apps
* Apps must trust the same identity source
* Reduces password fatigue and reduces help desk pain
* One identity means easier onboarding and offboarding

**Why it matters**
* One identity per user means fewer weak creds floating around
* Cleaner security because access moves with the user not separate accounts

**Takeaway**
* SSO makes life easier but it is only as strong as the first login

### Multifactor authentication (MFA)
**What it is**
* Adds an extra step to sign in so stolen passwords alone are useless
* Uses two or more of
  * Something you know
  * Something you have
  * Something you are

**Real examples**
* Password plus code from your phone
* Password plus fingerprint
* App notification plus PIN

**Why MFA matters**
* Stops most account takeover attempts
* Even if attacker has the password they stuck without the second factor

**Microsoft Entra MFA**
* Built in MFA service using phone call SMS app notification or other methods

**Takeaway**
* MFA is the biggest security upgrade you can turn on with the smallest effort

### Passwordless authentication
**Key points**
* Removes passwords entirely
* Uses a trusted device plus biometric or PIN
* Higher security and less user frustration
* Must register device before using it

**How it works**
* Device is something you have
* Biometric or PIN is something you are or know
* Combine them and you get a clean passwordless login

**Passwordless options**

**1. Windows Hello for Business**

      * Tied to your PC
      * Uses biometrics or PIN
      * Integrates with SSO and PKI
      * Great for anyone with a dedicated work laptop
      
**2. Microsoft Authenticator app**

      * Phone becomes your credential
      * Approve sign in with a notification plus biometric
      * Works across browsers and platforms
      
**3. FIDO2 security keys**

      * Physical hardware keys USB NFC or Bluetooth
      * Strongest form of passwordless
      * Unphishable
      * Follows open standard WebAuthn

**Passwordless flow diagram**
```bash
Trusted device  
    +  
Biometric or PIN  
    =  
Full authentication no password needed
```

**Takeaway**
* Passwordless makes logins safer and smoother at the same time

### Summary
**Passwords**
* Easy but weak

**SSO**
* One login to access everything

**MFA**
* Two or more factors for strong protection

**Passwordless**
* Best combo of security and convenience using device and biometric

**Final takeaway**
* Modern Azure identity is all about making security strong while making logins effortless

## Azure external identities

**Key points**
* External identity = anyone or anything outside your org that needs access  
* Microsoft Entra External ID lets you securely work with partners vendors suppliers customers  
* External users bring their own identity Google Facebook another Entra ID whatever they already use  
* Their identity provider handles their login and you control what they can touch in your tenant  

**Why this matters**
* You can collaborate across orgs without creating a bunch of local accounts  
* You stay in control of access while letting outsiders sign in with what they already have  
* Perfect for partner access B2B SaaS apps or full customer facing applications  

### External Identity Types

### B2B collaboration
* External users sign in with their own corporate or social identity  
* They appear in your directory as **guest users**  
* Works for Microsoft apps SaaS apps and custom enterprise apps  
* Classic partner access scenario  

**Takeaway**
* B2B collaboration = let outsiders in without giving them a whole new account  

### B2B direct connect
* Two orgs establish a mutual trust  
* No guest accounts created in your directory  
* Supported mainly for Teams shared channels  
* External users stay in their home org and still access your shared resources  

**Takeaway**
* B2B direct connect = seamless cross tenant collab no guest accounts no clutter  

### Azure AD B2C (Microsoft Entra External ID for customers)
* Used when youre building consumer facing apps  
* Lets customers sign in with social accounts or custom identities  
* You control the auth flow UX policies and access management  
* Completely separate tenant for customer identity  

**Takeaway**
* B2C = identity platform for apps that serve the public not enterprise partners  

### How external identities work

**Flow**
```txt
External user identity provider  
        ↓  
User signs in with their own creds  
        ↓  
Microsoft Entra ID handles access rules  
        ↓  
User gets access only to what you allowed  
```

**Why it works**
* You avoid managing credentials for outsiders
* You reduce attack surface
* You centralize access control
* You still get auditing MFA Conditional Access and governance on all external access

### Managing guess access
**Guest invitations**
* Admins or users can invite guests
* Guests appear as objects in your directory (B2B collab)
* You assign roles groups or app access like normal users
* Great for project based collaboration

**Access reviews**
* Ask guests or managers to confirm whether access is still needed
* Helps clean out old unused guest accounts
* Entra suggests which users look inactive
* After review you remove unneeded access

**Takeaway**
* External access is only safe if you keep it clean and reviewed

### Diagram
External identity options overview
```ascii
                External Identities
                  /      |       \
            B2B collab   |       B2C
                         |
                B2B direct connect

B2B collab
  guest accounts in your tenant

B2B direct connect
  no guest accounts shared trust only

B2C
  customer facing identity service
```

### Summary
**B2B collaboration**
* Let partners in with their own identity guest users created

**B2B direct connect**
* Two way trust no guest accounts Teams shared channels

**B2C**
* Identity for public apps customers and consumers

**Final takeaway**
* External identities let you bring people in without bringing in their problems
* They keep auth flexible access controlled and your environment protected

## Azure conditional access

**Key points**
* Conditional Access is the policy brain of Microsoft Entra ID that decides who gets in and how
* It looks at signals like user location device and app being accessed
* Based on those signals it can allow block or require extra verification like MFA
* Idea is simple let people work from anywhere but still protect the important stuff

**How it works**

**1. User tries to sign in to an app or resource** 

**2. Entra collects signals**  
   * who the user is  
   * where they are signing in from  
   * what device they are on  
   * what app they want to access  
   * risk level of the sign in  
   
**3. Conditional Access policy evaluates those signals**
  
**4. Decision is made**  
   * allow  
   * require MFA  
   * block
   
**5. Enforcement happens automatically on that sign in**

**Example logic**

* User on known device in trusted location  
  * allow access with no extra challenge  

* User on unknown device from risky country  
  * require MFA  
  * or fully block based on policy  

* Admin role user  
  * always require MFA no matter where they are  

**When to use Conditional Access**

* Require MFA only when needed  
  * for admins  
  * for access from outside office networks  
  * for sensitive apps like finance or HR  

* Force access through approved apps  
  * for example allow only specific mail clients  

* Allow access only from managed devices  
  * devices that are compliant and tracked  

* Block access from untrusted sources  
  * unknown locations  
  * unexpected countries  
  * risky sign in patterns  

**Conditional Access flow diagram**

```ascii
User sign in
      |
      v
+----------------------+
|  Collect signals     |
|  user device app     |
|  location risk       |
+----------------------+
      |
      v
+----------------------+
|   Policy decision    |
|  allow  MFA  block   |
+----------------------+
      |
      v
+----------------------+
|     Enforcement      |
|  action applied      |
+----------------------+
```

**Takeaway**
* If you understand how Conditional Access reads signals and chooses between allow MFA or block
you already thinking like the person who controls the front door of the whole cloud setup
the Entra version of a security guard who actually reads the guest list instead of just waving everyone in

### Summary
* Makes access decisions based on signals like user device location and app
* Lets you allow block or require MFA depending on the situation
* Protects org assets while keeping users productive
* More flexible than blanket MFA cuz it only challenges when needed
* Enforces rules during sign in flow automatically
* Perfect for blocking risky sign ins or unknown locations
* Can force access only from managed or compliant devices
* Can require specific approved apps for access
* Lets admins build “if this then that” security logic without custom code

## Azure role based access control

**Key points**
* Azure RBAC lets you control who can do what in your cloud setup  
* Based on the principle of least privilege give people only what they need and nothing extra  
* You assign roles instead of manually giving permissions one by one  
* Roles contain permissions and users inherit whatever the role allows  
* Makes access management way easier when teams grow  

### How RBAC actually works

When you give someone a role you are giving them a **bundle of permissions**  

Example  
* Reader role = can view but cant change anything  
* Contributor role = can create and modify but cant change access  
* Owner role = full control including access management  

Instead of managing permissions per person you assign them to a role or Azure AD group and boom everyone in that group gets the exact same access  

### What is a scope

RBAC is always applied at a **scope**  
A scope can be  

* Management group  
* Subscription  
* Resource group  
* Single resource  

If you assign a role high in the hierarchy it trickles down to everything under it  

### RBAC hierarchy

```ascii
Management Group
      |
      v
 Subscription
      |
      v
 Resource Group
      |
      v
  Resource
```

* Assign access at the top and everything below inherits it
* Assign access at the bottom and only that single thing gets it

### Example of inheritance
* Assign Owner at management group
  * user now controls all subscriptions all resource groups all resources
* Assign Reader at subscription
  * user can view everything in that subscription but cant modify anything

### Roles VS scopes diagram
```ascii
+----------------------------+
|        Role assigned       |
|   (Owner Contributor etc)  |
+-------------+--------------+
              |
              v
+----------------------------+
|          Scope             |
| mgmt group subscription    |
| resource group resource    |
+-------------+--------------+
              |
              v
+----------------------------+
|    Effective permissions   |
|   actions user can do      |
+----------------------------+
```

### How RBAC is enforced
* It only applies to actions that go through Azure Resource Manager
* That means portal CLI PowerShell REST API
* RBAC does NOT protect your application data only Azure resource access
* Uses an allow model
  * if a role gives you permission you have it
  * multiple role assignments stack together

Example
* one role gives you read
* another gives you write
  * → you got both read and write

**Takeaway**
* If you understand roles scopes and inheritance you already thinkin like the person who hands out the keys to the whole kingdom
* The cloud version of deciding who gets backstage passes and who stays behind the barricade

### Summary
* Give people only the access they actually need
* Use roles instead of manually assigning permissions
* Built in roles cover most jobs you dont gotta reinvent nothing
* Custom roles exist when you need that special sauce
* Permissions apply at a scope like management group subscription resource group or resource
* Higher level scopes pass permissions down the chain
* Add someone to a role and boom they get all the permissions tied to it
* Remove the role and poof access gone
* Azure RBAC only handles control plane not data plane  
  your apps still gotta protect themselves

## Zero Trust model

**Key points**
* Zero Trust assumes everything is hostile by default  
* No one gets a free pass not your users not your apps not even devices sitting inside your network  
* Every request is verified every time using identity device health location and risk  
* Access is granted with minimum permissions Just Enough Access and Just In Time  
* You operate on the mindset of assume breach and contain everything  

### What Zero Trust actually is

Old school security  
"if youre inside the network youre cool"

Zero Trust  
"nah fam show ID every time and even then you still only get what you absolutely need"

Instead of trusting a network location Zero Trust trusts signals identity and context  

## The three core pillars

### 1 verify explicitly
* Always authenticate  
* Always authorize  
* Use all signals  
  * user identity  
  * device health  
  * location  
  * risk score  
  * workload context  

### 2 least privilege access
* JIT Just in time access  
* JEA Just enough access  
* Reduce blast radius  
* No one gets more permissions than needed  

### 3 assume breach
* Always think something is compromised  
* Segment networks  
* Limit lateral movement  
* Encrypt everything end to end  
* Use analytics and threat detection everywhere  

## Zero Trust compared to old school security

```txt
Classic model
------------
Inside network = trusted
Outside network = untrusted
VPN = magic tunnel of trust
Attackers only get in if perimeter breaks

Zero Trust model
----------------
Nothing trusted by default
Every request authenticated
Every device checked
Permissions minimal
Assume attacker is already inside
```

### Zero Trust flow diagram
```txt
User or device request
         |
         v
  +--------------------+
  |   Verify identity  |
  |   device health    |
  |   location risk    |
  +--------------------+
         |
         v
+-----------------------+
|   Evaluate policy     |
|   JIT JEA conditions  |
+-----------------------+
         |
         v
+---------------------+
| Enforce decision    |
| allow deny or MFA   |
+---------------------+
```

### Why companies move to Zero Trust
* remote work
* mobile devices
* cloud apps
* attackers not respecting your old perimeter

Organizations need a model that works anywhere not just inside some office building

**Takeaway**
* Zero Trust is basically Azure saying trust nobody
verify everybody and lock down access like youre securing the last slice of pizza at a party
That mindset keeps your environment safe no matter where the users or devices roam

### Summary
* Trust nothing by default
* Verify every request like it owes you money
* Always authenticate and authorize with all signals
* Give users only the minimum access needed
* Use just in time and just enough access
* Assume breach all the time
* Segment everything so one slip dont wreck the whole house
* Encrypt end to end
* Use analytics to detect and react fast

## Defense in depth

**Key points**
* Defense in depth means you lock down your stuff in layers like an onion  
* Goal is to slow an attacker down and stop the breach before they touch your data  
* If one layer fails another layer steps in like backup dancers with hands  
* Azure gives tools for every layer from physical building locks to data encryption  

### Defense in depth layers
Visualize it like a big stack with your data chilling in the center and every other layer acting like a shield around it

**ASCII diagram**
```pgsql
          +----------------------+
          |   Physical security  |
          +----------------------+
          | Identity and access  |
          +----------------------+
          |     Perimeter        |
          +----------------------+
          |       Network        |
          +----------------------+
          |       Compute        |
          +----------------------+
          |     Application      |
          +----------------------+
          |        Data          |
          +----------------------+
```

### Physical security
**What it does**
* Protects buildings and hardware  
* Ensures attackers cant bypass everything and just yank your drives  
* Microsoft datacenters handle guards locks cameras biometric gates and all that  

### Identity and access
**What it does**
* Controls who can touch what  
* Uses SSO MFA audit logs and least privilege  
* You limit access and track changes so nobody sneaks in  

### Perimeter
**What it does**
* Shields your environment from big network attacks  
* Uses DDoS protection and firewalls  
* Filters malicious traffic before it even hits your network  

### Network
**What it does**
* Controls communication between resources  
* Enforces deny by default  
* Limits inbound and outbound internet access  
* Secures hybrid connections  

### Compute
**What it does**
* Ensures VMs and devices are locked down  
* Patch management endpoint protection secure access  
* Reduces risk from malware or misconfigurations  

### Application
**What it does**
* Keeps your apps clean of vulnerabilities  
* Secrets stored properly  
* Security added into dev workflow not tacked on later  

### Data
**What it does**
- Protects the thing attackers actually want  
- Encrypts and controls access to data in DBs storage accounts and SaaS apps  
- Meets compliance and regulatory requirements  

### Summary
* Defense in depth stacks multiple layers of protection  
* Each layer covers the one before it  
* Designed to slow attackers down and alert defenders  
* Azure provides tools across every layer to secure your apps and data  

## Microsoft Defender for Cloud

**Key points**
* Defender for Cloud is your all in one security posture and threat protection platform  
* Monitors Azure onprem hybrid and multicloud environments  
* Helps harden resources identify vulnerabilities and detect threats  
* Gives recommendations secure score and alerts so you always know what needs fixing  
* Azure native so setup is instant for most Azure services  

### What Defender for Cloud does
* Continuously evaluates your security posture  
* Finds misconfigurations and vulnerabilities across compute data and storage  
* Protects Azure PaaS services networks SQL and containers  
* Extends protection to hybrid servers through Azure Arc  
* Extends protection to AWS and GCP with CSPM and Defender plans  
* Detects threats and gives detailed alerts and remediation guidance  

## Where it protects

### Azure native services
* App Service SQL Storage and more  
* Threat detection and anomaly detection  
* Vulnerability assessments for SQL and storage  
* Just in time VM access to reduce open ports  
* Adaptive application controls to limit what runs on a VM  

### Hybrid resources
* Uses Azure Arc to onboard non Azure machines  
* Same alerting and posture checks for onprem servers  
* Prioritized threat intelligence based on your environment  

### Multicloud AWS and GCP
* CSPM assessment for AWS  
* AWS resources appear in secure score  
* Defender for Containers protects Amazon EKS clusters  
* Defender for Servers protects EC2 instances  

## Core model: Assess Secure Defend

### Continuously assess
* Vulnerability assessment for VMs containers and SQL  
* Native integration with Defender for Endpoint  
* Regular scans across your stack  
* Everything visible inside Defender for Cloud  

### Secure
* Security policies through Azure Policy  
* Azure Security Benchmark as the baseline  
* New resources scanned instantly  
* Secure score shows posture health  
* Recommendations grouped into security controls  

### Defend
* Security alerts with remediation steps  
* Kill chain analysis for full attack story  
* Automated responses with logic apps  
* Threat protection for VMs SQL containers networks  

## Defender for Cloud workflow diagram

```txt
          +----------------------------+
          |   Continuously Assess      |
          | vulnerabilities misconfigs |
          +-------------+--------------+
                        |
                        v
          +----------------------------+
          |          Secure            |
          | apply benchmarks policies  |
          | improve secure score       |
          +-------------+--------------+
                        |
                        v
          +----------------------------+
          |           Defend           |
          | alerts threat detection    |
          | kill chain correlation     |
          +----------------------------+
```

**Takeaway**
* Defender for Cloud monitors Azure onprem and multicloud
* Shows what is wrong how risky it is and how to fix it
* Secure score gives one glance at your overall security health
* Threat detection uses correlation not just raw alerts
* Works with AWS and GCP for one unified security view

## References
https://learn.microsoft.com/et-ee/training/modules/describe-azure-identity-access-security/
