<img width="931" height="625" alt="image" src="https://github.com/user-attachments/assets/d70c3697-f4e1-4ab0-9e56-5acf03132bf0" /># Lab 2b: Providing private storage for internal company documents

**Job Skills**

* Creating storage account for private documents within the company.
* Configuring respective redundancy for storage account.
* Configuring shared access signature so partners would have restricted access to a file.
* Backing up public website storage.
* Implementing lifecycle management to move content to the cool tier.

## Part 1: Creating storage account and configuring high availability

1. Created new storage account `private18` and added it to my previously created RG group `StorageOnly`:

<img width="873" height="659" alt="image" src="https://github.com/user-attachments/assets/a9e02834-4e0e-4263-828e-d34b74fd662b" />

Left everything else at default. Went to `data management` > `redundancy` to make sure GRS was selected.

Switched from RA-GRS to GRS and looked over primary and seconday location info:

<img width="676" height="552" alt="image" src="https://github.com/user-attachments/assets/ebae4e35-a7d2-4e70-8462-1942fceed105" />

GRS was now selected, saw that primary region was set to norway east and 2ndary as norway west.

Saved changes.

## Part 2: Creating storage container, uploading a file and restricting access to the file

1. In the `private18` storage account went to `data storage` > `containers` and made a new container named `private`. 

By default when creating a new container the access level is set to `Private (no anonymous access)` this can be changed as seen in my previous lab by going to overview page of the storage account and under properties enabling blob access level. 

2. Next i uploaded a file to the container and tried copy pasting the URL of the file into a new browser tab, the file shouldnt display and i should recieve an error because access to the file is set to private:

<img width="949" height="265" alt="image" src="https://github.com/user-attachments/assets/051801d9-7a49-4bc7-815c-a5b278db5dc8" />

The access level is working.

3. Next i configured and tested shared access signature (SAS) in case an external partner would have required read and write access to the file for at least the next day or so.

So within the `private` container i selected the file i had uploaded and opened it up. Then i moved to `Generate SAS` tab:

<img width="488" height="218" alt="image" src="https://github.com/user-attachments/assets/8944f347-421b-40d4-a6f4-3b91992f6fd7" />

Under `Permissions` read value was already selected. Made sure that start and expiry date was for the next 24 hours then > selected `Generate SAS token and URL` at the bottom.

<img width="929" height="117" alt="image" src="https://github.com/user-attachments/assets/c6dc355f-c21c-47fb-b773-1635941510a5" />

Copied the Blob SAS URL and pasted in the other tab:

<img width="846" height="672" alt="image" src="https://github.com/user-attachments/assets/48e37c86-03f5-48aa-9cfe-1c25599f26f2" />

## Part 3: Configuring storage access tiers and content replication

To save costs after 30 days its wise to move blobs from hot tier (costly) to cool tier (cheaper).

1. Went back to the storage account and in the overview page under properties i could see the access tier was set to `hot`:

<img width="322" height="272" alt="image" src="https://github.com/user-attachments/assets/2088974e-bf75-4ee5-9a9a-4298164cce1c" />

I could have switched tiers right now, but that would defeat the purpose of lifecycle management.

Went to `data management` blade and then > `Lifecycle management` > selected `add rule`.

My goal was to create a rule for the storage account that after 30 days the blobs would be moved from hot tier to cool tier to save on costs and since this data wouldnt be accessed often and would just sit there most of the time. 30 days allows to maximize savings. However there is trade off between hot and cool tier. By switching to cool tier i would pay less to keep the data, however if i would pay more to read or write it compared to hot tier.

Anyway by adding the rule to move data after 30 days to cool tier im automating the process. New data is moved to hot tier / old and unused is being moved to cool.

2. Filled out the `details`:

<img width="780" height="480" alt="image" src="https://github.com/user-attachments/assets/f18bcf40-0cee-4789-94ef-7e8e2d15c501" />

Moved to `base blobs` tab where there was a simple "if/ then" logic and filled it:

<img width="784" height="521" alt="image" src="https://github.com/user-attachments/assets/7df4e60a-a744-4247-9ca6-1ba1497044b3" />

So now i had set the conditions that if blobs were last modified more than 30 days ago, then they would be moved to cool tier. I also noticed i could add more conditions, for example i added a condition that if blobs were created more than 120 days ago they would be moved to archive instead. However i got curios about option `Skip blobs that have been rehydrated in the last... days`. 

I learned that rehydration is costly to do and what it does is that it wakes up a file that has already been archived. Apparently its a costly procedure and takes a while. So this option basically enables you to pause it. Basically say you have realised you need specific file from the archive, you rehydrate it. Then if you check this rule it means that it keeps all rehydrated blobs within the specified timeframe (lets say 15 days) still active and doesnt shove them back instantly to archive, it gives you moment of grace basically.

Anyway i left that checked so now i created the following rule logic:

Last modified blobs more than 30 days ago would go to > Cool storage. If base blobs were created more than 120 days ago > they would move to archive. Subsequent option > Skip blobs that have been rehydrated in the last 15 days, to give me grace period (even tho i had new files so really this option does nothing, but it does future proof):

<img width="795" height="866" alt="image" src="https://github.com/user-attachments/assets/e02084f8-642f-4e75-9b1a-b6d7071a0e03" />

Selected `add`

Noticed there was `code view` option where i could copy this rule in JSON format for later automation if needed.

3. Needed to create backup for public website files to another storage account. So went back to the storage account and created new container and named it `backup`, used default values.

4. Navigated to `publicwebsite71` storage account i created prior, then > `data management` > `object replication` > `create replication rules` from the top.

Set the destination storage account as `private18` since it contains my `backup` container where i want my data to be replicated to.

Set the source container to public (only container in there anyway) and as the destination container `backup`.

<img width="931" height="625" alt="image" src="https://github.com/user-attachments/assets/072edb16-6a37-4049-ac50-173062b2c3aa" />

Created the rule. 

I then checked if it was working, so i went to `publicwebsite71` storage account, opened up `public container`. Uploaded a file there and then checked my backup container in the `private18` to see if it had replicated the file in there:

<img width="697" height="259" alt="image" src="https://github.com/user-attachments/assets/5b2c2fbb-1c7a-4ad0-b211-a21f9f3a4b22" />

I then went back to `publicwebsite18` > `object replication` > found my created rule and edited settings that instead of copying new files that are uploaded to public container it will copy over everything currently inside the public container. 

Noticed it takes a few minutes before the replication is actually complete so i waited around 6 minutes for this to take effect.

<img width="692" height="360" alt="image" src="https://github.com/user-attachments/assets/fb96315e-ee16-41be-9dbb-a472a855a778" />


Next i performed cleanup by deleting resource group `StorageOnly`

### Summary

### Key Takeaways

