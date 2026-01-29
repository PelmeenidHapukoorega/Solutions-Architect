# Lab 2a: Providing storage for the public website

**Job Skills**

* Creating storage account with high availability
* Ensuring storage account has anonymous public access
* Creating blob storage container for the website documents
* Enabling soft delete so that files could be easily restored
* Enabling blob versioning

## Part 1: Creating storage account with high availability

1. Created new storage account to an existing resource group from previous lab. Named it `publicwebsite71` and left everything at default: 

<img width="637" height="113" alt="image" src="https://github.com/user-attachments/assets/00b2e9c5-a78c-4a16-9f40-f6aa1c8bde18" />

2. Made sure that `read access geo redundant storage` was selected under `Data management` > `redundancy` blade.
<img width="665" height="67" alt="image" src="https://github.com/user-attachments/assets/145dc645-cc22-42b0-8fa8-895b524223c4" />

Then went to `settings` >  `configuration` and ensured that `allow blob anonymous access` setting was enabled: 

<img width="228" height="64" alt="image" src="https://github.com/user-attachments/assets/77505b2a-6207-4d18-9e12-8d90042c870a" />

This allowed me to configure container ACLs (access control list) to allow anonymous access to blobs within the storage account.

## Part 2: Creating blob storage container with anonymous read access

1. Went back to my created storage account `publicwebsite71` then went to `Data storage` section > `Containers` > Created new container `public` and enabled read access of blobs only so the customers could view the images without being authenticated:

<img width="419" height="288" alt="image" src="https://github.com/user-attachments/assets/db4bef4e-37f4-445c-aee9-47ca41096c9b" />

2. Uploaded 3 files to the container:

<img width="675" height="319" alt="image" src="https://github.com/user-attachments/assets/21a308b5-9192-416d-b1eb-a0336c6336b9" />

3. Tested URL for each uploaded file by selecting the file and then from its overview page, copy the URL and paste it in the browser:

<img width="808" height="784" alt="image" src="https://github.com/user-attachments/assets/6cc875e5-24dc-4386-a2d6-8c865c9af908" />

<img width="604" height="864" alt="image" src="https://github.com/user-attachments/assets/ffdc3081-f5fa-4ac7-9e29-f98b649abc82" />

<img width="663" height="764" alt="image" src="https://github.com/user-attachments/assets/34915be0-dcbc-4c44-ad05-579c46147214" />

I had successfully configured storage container with anonymous read access. This pretty much meant that anyone with a link could view these since i made sure access level was set to public which gave me the anonymous read access for blobs only.

## Part 3: Configuring soft delete

1. Went to `storage account` overview page, scrolled down and looked for `blob soft delete` under properties:

<img width="298" height="598" alt="image" src="https://github.com/user-attachments/assets/986b308a-807f-48b4-8f6b-c98874281831" />

Ensure that `enable soft delete for blobs` was checked and changed the setting for `keep deleted blobs for` from 7 days to 21 days. I also noticed this could be done for containers too.

This means that the system would keep any deleted blobs (files) this setting basically defines the timeframe for how long blobs are kept before being permanently deleted, same thing applies to containers themselves should you accidentally delete one. 21 days is optimal.

Saved changes, then went back to public container and deleted a file there on purpose in order to recover it.

Before the deletion it hit me with a warning whether i wanted to go through with file deletion and i noticed that it specifies exactly for how long the current file would be recoverable:

<img width="320" height="326" alt="image" src="https://github.com/user-attachments/assets/e0e3926f-df98-48e7-a401-6cecf0107b82" />

Then i used filtering from the top right and applied `show active and deleted blobs`:

<img width="694" height="275" alt="image" src="https://github.com/user-attachments/assets/40614840-f258-49f5-a04e-9aab4497e687" />

To get the file back all i had to do was select `...` of the deleted file and select `undelete`:

<img width="672" height="224" alt="image" src="https://github.com/user-attachments/assets/5e0c11a8-7368-44c3-8fcf-0f3e17a748d9" />

And there it was again.

## Part 4: Configuring blob versioning

1. On the overview page of `storage account` selected `versioning` under properties and enabled it:

<img width="307" height="506" alt="image" src="https://github.com/user-attachments/assets/20af8459-a076-44a2-86ec-bec1f547ab1a" />

I noticed 2 settings under versioning:

<img width="650" height="251" alt="image" src="https://github.com/user-attachments/assets/c2922869-0849-4b8b-b2c2-355ef2d635c5" />

I decided to go with `Delete versions` and set it to 3 days. I have no interest in keeping the same file with different edits and then later discovering i have so many versions of the same file. 

So by using delete versions and adding time frame to 3 days meant that i could upload different/newer versions of the file without having to manually delete the older ones. Delete versions option deletes all your older versions of the same file while leaving the newest version untouched. Its great for maintaining order and not having to worry about making 100 edits/uploads and then do manual labor of deleting older files.

I also noticed that when i applied the rule and later wanted to change it i couldnt do it in the overview page. I had to go to `Lifecycle management` and then delete the rule to see those options again.

In any case, when i uploaded the same file to container it asked for me to overwrite the existing one. Then i remembered that `delete versions` should delete all prior versions in 3 days time, but where could i actually see those versions?

For that i needed to go to `container` open up the exact file, then from the top pick versions and voila:

<img width="947" height="365" alt="image" src="https://github.com/user-attachments/assets/fe5d9df4-cf0b-48bc-b5e4-e6f165c9eacd" />

### Summary

### Key Takeaways

* Blob storage is optimized for storing massive amounts of unstructured data (images, videos etc). Unstructured means data that doesnt adhere to particular model or definition like text or binary that would go to tables for example.
* Blob soft delete protects an individual blob, snapshot or version from accidental deletes or overwrites by maintaining the deleted data in the system for a specified period of time.
* Blob versioning maintains previous versions of a blob. When its enabled you can restore earlier version of a blob to recover data if its modified or deleted.
* When container is configured for anonymous access any client can read the data in that container.
