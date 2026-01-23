# Lab 2b: Managing governance via Azure policy

**Job skills**

* Creating and assigning tags via portal.
* Enforcing tagging via Azure policy.
* Applying tagging via Azure policy.
* Configuring and testing resource locks.

## Part 1: Assigning tags

1. Created new resource group and named it `104RG` and set the location for it `East US`
2. Under `Tags` tab i added a name for tags and default value before enforcing. Selected `Reviews + create`:

<img width="376" height="385" alt="image" src="https://github.com/user-attachments/assets/1865d2ea-96e1-4135-97e1-484485ab4316" />

## Part 2: Enforcing tagging through Azure policy

1. Searched for `Policy` in the portal.
2. On the `Authoring` blade went to `Definitions`. Searched for `Require a tag and its value on resources` built in policy and selected `Assign`
3. In the `Basics` tab i needed to define scope aka where the policy would be applied. Clicked the `...` dots next to empty scope tab then Selected my subscription and resource group respectively where the policy will be applied to:

<img width="591" height="330" alt="image" src="https://github.com/user-attachments/assets/2e4eccf0-173f-4b9b-8894-321e0ecd1866" />

Management group was unchecked because i wasnt creating a policy for it, and if i were to add policy to that then in hierarchy terms it would apply to all other subscriptions and resource groups under the management group. In this case i needed policy for speficic RG group under specific subscription even tho i have an option of specifying exlusions for subscriptions, rg groups and resources.

4. Under `Basics` tab provided value for assignment name and description and made sure `Policy enforcement` was enabled.
5. Under `Parameters` i added tag name i gave to resource group as well as the value i assigned to it.
6. Made sure `create a managed identity` option was unchecked under `Managed identity` tab.

I did this because managed identities are for "Active" policies not "Passive" blocks. Resource tag policy uses deny effect because it only evaluates requests and doesnt modify existing data. This taught me that i should only provision identities when a policy is designed to perform remediation tasks which helps ensuring that i dont grant unnecessary administrative power to the platform.

7. Reviewed settings:

<img width="881" height="879" alt="image" src="https://github.com/user-attachments/assets/064bef1e-5108-4558-a6a2-95a8fa9fec77" />

And selected `Create`

8. I needed to check if the policy actually works, so i went to `storage accounts` and created a new one named `pentagonsdirtybox`, chose LRS for redundancy because its cheapest just in case. Then selected `reviews + create`

As expected i was denied:

<img width="946" height="365" alt="image" src="https://github.com/user-attachments/assets/f73459ae-adaf-42b5-8f86-66cb62ed120b" />

Which meant that the policy itself works.

## Part 3: Applying tagging through Azure policy

1. Went back to `Policy` and selected `Assignments` from the left panel and then selected `...` next to my assigned policy and used `delete assignment`.
2. In the same landing page i assigned new policy `Inherit a tag from the resource group if missing`
3. Under `Parameters` added tag name Cost Center as before. Then under `Remediation` the option now was unblocked because this policy supports remediation tasks, so i checked box `Create a remediation task` and `Inherit a tag from the resource group if missing` was already auto selected.

Since remediation task was enabled it meant that managed identity was also required because policy definition included `Modify` effect.
4. Checked `Managed Identity` tab and it was auto selected. I noticed that there is an option to add managed identity as the user instead of letting the system take care of it.

* **System assigned:** Is tied to specific resource, its created when enabled on a resource and automatically deleted if the resource is deleted. Its 1-1 relationship aka 1 resource has one system assigned identity. Its best used on standalone resources that need to talk to key vault, database and they dont share identity with anything else, ideal for security reasons.
* **User assigned:** Has its own lifecycle, when created it stays until its manually deleted regardless of resource attachment. Its many to many relationship. It can be assigned to multiple VMs and functions. On the other hand a single resource can have multiple user assigned identities. Its great for scaling. For example lets say i have 10 vms in a cluster that all need specific storage account, i can create 1 user assigned identity, give it permission and then stick it on all 10 VMs.

Final review before creation:

<img width="895" height="886" alt="image" src="https://github.com/user-attachments/assets/f125c6f8-6e99-415c-86fc-3424d8f5a791" />

Note: It can take between 5-10 mins for the policy to take effect.

Selected `Create` and it took in reality just a couple seconds to deploy.

5. Went back to `Storage accounts` to create one and see if the policy lets me create the storage account.

Success:

<img width="904" height="539" alt="image" src="https://github.com/user-attachments/assets/516b6352-cc0a-4fc3-8dd7-fb611f4c951b" />

6. Went to `Tags` and selected the created tag `Cost Center : 000` to see whether the provisioned storage account shows up under there.

<img width="910" height="382" alt="image" src="https://github.com/user-attachments/assets/4af7991d-dd56-401c-9082-a04518b97982" />

It did. This meant that policy worked perfectly, any new resource created would be automatically signed Cost center tag when provisioned in to resource group with defined tag and would show up in `Tags`. This makes it easier to group resources into speficically defined resource groups and additionally it makes it easier to filter for resources and can be looked up fast for cost management if needed. 

## Part 4: Configuring and testing resource locks

1. Selected my resource group `104RG` > Settings from the left panel > `Locks` > `Add` from the top. Filled it out and for lock type picked `Delete` which stops the resource group from deletion. To delete RG group later, you would have to delete the lock on it.
2. For humoring reasons tried to delete `104RG`:

Worked out as expected:

<img width="352" height="154" alt="image" src="https://github.com/user-attachments/assets/b783539f-b9ae-4769-9d79-d74dcc540937" />

3. Deleted the lock and did cleanup.

### Summary 

Interesting lab this time around. Learned how to create assign tags to resource groups to tracks costs and metadata easily. In order to enforce the tags i had to use Azure policy to assign the `Require a tag and its value on resources` definition. This worked as a passive block using deny effect. It was pretty cool to see that once the policy was live then trying to create storage account without the tag led to validation failing. 

Practiced applying tags automatically by switching to the `Inherit a tag from the resource group if missing` policy. This was the active part and required Managed identity because it used a remediation taks with `Modify` effect to fix resources for me. Learned the difference between `System assigned` and `User assigned` which was that system assigned is 1-1 relationship for each resource seperately and user assigned is many to many aka the opposite.

Then i set up resource lock which was the easiest part of the lab but i wanted to test it out by trying to delete RG group if the lock type was set to `Delete` and `Read` and yeah its bulletproof, you cant simply delete RG group like that, you need to remove the lock, luckily removing it wasnt complex at all. This lab taught me how to easily group resources for cost management and keep everything relatively safe while provisioning new stuff.

### Key takeaways

* Azure tags are metadata that consist of key value pairs. Tags describe resources in the environment. Tagging enables to label resources in logical manner.
* Policy establishes conventions and rules for resources. Policy definitions describe resource compliance conditions and the effect to take if a condition is met. Condition compares resource property field or a value to required value. There are many built in definitions and policies can be customized.
* Policy remediation task feature is used to bring resources into compliance based on definition and assignment. Resources that are non compliant to a `Modify` or `deployIfNotExist` definition assignment can be brought into compliance using remediation task.
* Resource locks can be configured on subscriptions, resource groups or resources. Locks prevent all of those from accidental user deletions and modifications. Locks override any user permissions.
* Policy is pre deployment security practice. RBAC and resource locks are post deployment security practice.
