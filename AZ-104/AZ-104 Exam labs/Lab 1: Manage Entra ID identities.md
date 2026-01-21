# Lab 1: Managing MS Entra ID identities

Job skills:

* Creating and configuring user accounts.
* Creating groups and adding members.

1. Went to portal and searched for `Microsoft Entra ID` section.
2. On the landing page i opened up `Manage tenants` from the top to become familiar with it.

**Note to self**

Tenant is specific instance of Entra ID containing accounts and groups, you can create more tenants and switch between them at will.

3. Went to `Manage` and selected `Users` then `New user` from drop down and clicked `Create new user`.
4. Filled out the information for both `Basic` and `Properties` tab and added usage location for the user as well as department where said user works at ass well as Job title to easily distinguish role from users list later as well as for filtering reasons.

Selected `Review + create`:

<img width="612" height="694" alt="image" src="https://github.com/user-attachments/assets/ebfa4ae2-2183-4eae-88af-6b2ebe2e420c" />

Success:

<img width="825" height="58" alt="image" src="https://github.com/user-attachments/assets/6d5b7278-4b10-4b9b-9272-42d27eb0e98d" />

## Part 2: Inviting external user

1. In the `New user` drop down selected `Invite external user` and filled out the info:

<img width="662" height="742" alt="image" src="https://github.com/user-attachments/assets/25625f4f-aecd-4f6b-aaf4-16d8f4e5530b" />

Proceeded with creation.

Success:

<img width="1044" height="44" alt="image" src="https://github.com/user-attachments/assets/a83a2d54-f520-4219-8336-6d315f0cd62d" />

Checked for invite on my personal email and logged in with the link provided, it asked for single sign in code and once i entered that i was in:

<img width="874" height="440" alt="image" src="https://github.com/user-attachments/assets/f08816ed-dd39-496e-bd56-415c7678614e" />

# Task 2: Creating groups and adding members

1. From Entra ID landing page under `Manage` selected `Groups` and then `New group` from the top. For `Group type` i selected `Security` because i was making a group for IT administrators who access critical data, then filled it with info:

<img width="777" height="499" alt="image" src="https://github.com/user-attachments/assets/b4240807-462e-418d-964b-849c0d4f3b8b" />

I also set my own account as the owner of the group and added regular user Urmas and external user Vambola as members by clicking on respectively on `Owner` then looking for my account and then `Members` and adding Urmas and Vambola.

Clicked create.

Checked for groups existance:

<img width="1023" height="741" alt="image" src="https://github.com/user-attachments/assets/ee5cecf2-88b8-4c65-a568-cc883030b04a" />

I could see that 1 owner was assigned and 2 members, wanted to check who was who so i went under `Owners` from `Manage` blade and confirmed myself as the owner of the group:

<img width="1059" height="299" alt="image" src="https://github.com/user-attachments/assets/0e45e85b-0c0d-4f4d-b9ae-055afc27486f" />

Then checked under `Members`:

<img width="1308" height="391" alt="image" src="https://github.com/user-attachments/assets/460fee81-6760-459b-8b11-9c8b85e4955d" />

Success.

### Summary

Quick and simple lab, nothing complex when it comes to creating users and groups. Learned how to invite external users and how the process for external users to get in looks like so that was pretty cool. Additonally noticed that when i wanted to delete my own user prior i couldnt do that because it was assigned global administrator role and also was a group owner, so in order to delete owners in this scenario you need to add a different owner for transfer.
