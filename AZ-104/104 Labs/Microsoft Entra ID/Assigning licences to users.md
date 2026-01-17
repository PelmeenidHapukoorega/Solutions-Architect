Lab 1: Creating new Microsoft Entra ID, security group and assigning license to a group.

1. Went to Microsoft Entra admin centre
2. Selected `Users` from the left panel and then `All users`
3. Clicked `+ New user` and then `create new user` and filled it with made up info
<img width="885" height="883" alt="image" src="https://github.com/user-attachments/assets/5c8b17ce-7c54-47a7-9260-7fb78aeb5e1f" />

Copied the generated password to notepad for testing and left the properties empty and selected `review + create`.

Last review of configs:

<img width="587" height="910" alt="image" src="https://github.com/user-attachments/assets/a7ab60e8-aa46-48be-b856-ee272bb7dd9d" />

Success:

<img width="592" height="57" alt="image" src="https://github.com/user-attachments/assets/c84a016d-6024-48de-a36b-2ea8fe6a149d" />

4. Created Security group by going to `groups` selecting `all groups` from the left panel and then `new group` from above selection.

5. Filled it with information and added my subscription user as the owner of the group and newly created user `Vambola` as a member of the group

<img width="726" height="529" alt="image" src="https://github.com/user-attachments/assets/6c4f8d8c-843f-4592-a6fd-95b3e53be3a3" />

Verified it under `all groups`

<img width="1140" height="107" alt="image" src="https://github.com/user-attachments/assets/56ca33d2-3ddb-4259-8550-25ee172a1c8c" />

6. Went to marketing window and from the left panel under `manage` selected `licenses`

Since no licenses are assigned to the group the portal will look empty with a direct link to `admin.microsoft.com` to select a license

<img width="902" height="393" alt="image" src="https://github.com/user-attachments/assets/27fe7511-97c5-48c8-b4b7-d522be0d24c8" />

7. I couldnt log in with the entra ID i was using Azure with since admin centre apparently blocks all personal emails.

I went back to Entra ID and created Admin user for myself, then under `assignments` tab i went to `role assignments` and gave it `global administrator` permissions since admin centre doesnt let anyone without admin privileges handle identity and billing.

After creating admin role i managed to log in with ease and needed to set up MS authenticator by default for it before i could move on to licensing, so i scanned the QR code etc and logged in.

I noticed i couldnt add license since the new `admin` user didnt have any groups associated with it. So i went back to `Entra ID` opened up the `marketing` group i had created and added `admin` as the new owner, removed my personal account and added that as another member for `marketing` group.

After making that switch the group showed up in admin centre:

<img width="856" height="420" alt="image" src="https://github.com/user-attachments/assets/3e7f0dbb-36d2-4dbe-a081-747fa942af21" />

Unfortunately could not continue with the lab since Microsoft Entra ID free doesnt support Group based licensing and is considered a premium feature. However if i did the process would have been selecting a license from `license` landing page, selecting the wanted license, then selecting groups from the list near the top screen, and in the `groups` page click `+ assign license` then searching for `marketing` (the group i created) and then select `assign` at the end of dialogue. Confirmation message should pop up.


I tried to delete users as alternative and how to restore users if needed. Deleting is straightforward, you go `users` select a user you want to delete and then from top menu click `delete`. 

From the left panel there is a section called `deleted users` so i clicked on that. Saw my deleted `vambola` user:

<img width="884" height="369" alt="image" src="https://github.com/user-attachments/assets/d83908dd-8faa-414c-9a1c-30c69596d31d" />

Selected it and from top menu clicked `restore user` and that was it, now i knew the user would be restored but wanted to check if it reappears in the group i had assigned it to prior:

<img width="883" height="472" alt="image" src="https://github.com/user-attachments/assets/6f28e941-6c52-435a-8417-cb422fc2949a" />

Checks out, Entra restores users with full properties it was given prior to deletion.


**Important!**

If you delete a user the account remains in suspended state for 30 days, in that window the user account can be resotred along with its properties, after the window passes the permanent deletion process is automatically started. Restorable users can be viewed under Microsoft Entra ID user interface.


### Summary



