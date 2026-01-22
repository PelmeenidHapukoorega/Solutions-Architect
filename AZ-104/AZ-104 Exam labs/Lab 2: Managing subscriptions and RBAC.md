# Lab 2: Managing Subscriptions and RBAC

**Job skills**

* Implementing management groups.
* Reviewing and assigning built in Azure roles.
* Creating custom RBAC roles.
* Monitoring role assignments with the Activity log.


## Part 1: Implementing management groups to organize and segment subscriptions

1. Went in portal to `Entra ID` landing page and from the left pane `Manage` selected `Properties`. Then i enabled elevated access for my user:

<img width="643" height="161" alt="image" src="https://github.com/user-attachments/assets/cb407ee5-b910-4212-b26d-1e95376ee7db" />

I enabled it because it allows me to manage all subscriptions and managemenet groups within the tenant.

2. Searched for `Management groups` in the search bar and then selected `Create` from the top.
3. Filled it with values:

<img width="735" height="267" alt="image" src="https://github.com/user-attachments/assets/943b5fae-be55-4372-9971-259f15ba9227" />

Selected `submit` and then verified the groups existance:

<img width="660" height="138" alt="image" src="https://github.com/user-attachments/assets/9c9e1a23-3795-4388-81f4-baa6f5fd9c0a" />

I noticed that this window view shows the hierarchy perfectly, tenant group is the main one then if clicked open it shows all subscriptions and created management groups under the tenant for ease of view.

## Part 2: Reviewing and assigning built in Azure role

1. Quickly created a Helpdesk group in Entra ID for this lab in order to assign proper roles later on using prior lab knowledge.
2. Went back to `Management groups` and selected `Management group 1` i had created, then from blades selected `IAM` > `Add` from the top > `Add role assignment` > Searched for `Virtual machine contributor` role to give to helpdesk group. 
3. Filled out values for role assignment > Selected `Select members` under `Members` option and added my helpdesk group:

<img width="917" height="580" alt="image" src="https://github.com/user-attachments/assets/f95f3418-341a-4cf8-8b22-34bb7750c4cf" />

Selected `Review + create` and got it done:

<img width="341" height="76" alt="image" src="https://github.com/user-attachments/assets/da6ab3b2-061e-4278-9fbc-652f51a4fde7" />

4. Went back to IAM panel and `Role assignments` to confirm whether the group was assigned the correct role:

<img width="959" height="135" alt="image" src="https://github.com/user-attachments/assets/c6ab25bd-b4ae-423a-a618-10b2a26f7bce" />

## Part 3: Creating custom RBAC role

1. Navigate back to IAM selected `+ Add` > `Add custom role`. Gave it a name and description then under `Baseline permissions` noticed 3 options.

You can either:

1. Create a role from scratch.
2. Clone a role by looking for it in the list.
3. Start from JSON.

For now i went with Cloning option > Searched for `Support request contributor` and selected that.

2. Went to `Permissions` > Selected `+ Exlcude permissions` from the top and searched the field for `.Support`. Only 1 option came up which was `Microsoft.Support` > Selected that.
3. In the permission list i checked `Other: Registers support resource provider` and selected `add`.

This was done because Azure resource provider is a set of REST operations which enables functionality for specific Azure service. Having Helpdesk have this capability gives them ability to enable varios Azure services that havent been vetted for security, could incur costs and would pose security risks.

4. Went to `Assignable scopes` to make sure my management group 1 was listed where it will be applied to:

<img width="928" height="145" alt="image" src="https://github.com/user-attachments/assets/0c59011e-f96c-43f4-8170-2f61fca6088e" />

5. Reviewed the JSON for `Actions`,`NotAction` and `AssignableScopes` to make sure everything was done as intended:

<img width="789" height="562" alt="image" src="https://github.com/user-attachments/assets/d81a7e06-9660-40e2-b6e5-6297f9a9a6ae" />

Sidenote: In JSON tab you can still edit the code by adding more scopes, permissions etc if needed.

Final review:

<img width="608" height="657" alt="image" src="https://github.com/user-attachments/assets/908853df-ebad-4f52-84b5-7ce8c543eaae" />

Selected `Create`

<img width="760" height="109" alt="image" src="https://github.com/user-attachments/assets/970cd403-ec1b-4544-b2b6-ab23b2406231" />

With this i had created a custom role and assigned it to scope aka management group 1.

## Part 4: Monitoring role assignments with Activity log

1. Went back to `Management groups` and from the balde selected `Activity log`. Added filter `Operation` aka action and searched for `Role`> Selected `Create role assignment` from the list for easier filtering for role creation specifically.

<img width="1019" height="211" alt="image" src="https://github.com/user-attachments/assets/dd9996b5-0350-4260-ab32-45a2fb26201b" />

Opened up the first one and selected `create role assignment` to view details as in where the role was created and which role exactly:

<img width="827" height="330" alt="image" src="https://github.com/user-attachments/assets/d29660a0-0fcb-4a43-a1bf-15c3fe8e854b" />

With this, the lab was done and performed cleanup:
```PWSL
Remove-AzManagementGroup -GroupName "MG1"
```

Note to self: Tried to remove the group with its full display name `Management group 1` but in reality it requires the ID of the group in order for the command to work.

<img width="548" height="57" alt="image" src="https://github.com/user-attachments/assets/f560b029-dc19-4476-9fb6-213b54d7f4ea" />


### Summary

