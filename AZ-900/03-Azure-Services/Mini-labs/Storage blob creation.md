## Mini lab: Creating a storage blob

1. Signed into portal.
2. Selected `Create a resource` in main menu.
3. Under popular Azure services picked `Storage account` and pressed `create` under it.
4. Filled the basics tab with following:

<img width="870" height="348" alt="image" src="https://github.com/user-attachments/assets/9eee8785-d376-40cd-b13f-cb95ce92ece8" />

5. Named it `pelmeenikauss`
6. Went to `Advanced tab` and checked `Allow enabling anonymous access on individual containers`. Why? In order to see if potential "Clients" could view my inserted images and not just me.
7. Selected `review and create`.
8. All settings met my requirements then pressed `create`
9. Deployment took about 15 seconds.
10. Went back to main menu and under `Resources` clicked on my storage account `pelmeenikauss`.
11. Then from left panel menu scrolled down and clicked `Containers`.
12. Pressed `+ add cotainer` to create a new container where i could upload an image.
13. Named it `pelmeenid` and set access level to `anonymous read access for blobs only` since i want randoms to have access to it.
14. Opened the container `pelmeenid` and clicked `upload` at the top to upload an image.
15. Uploaded my image.
16. Clicked on the uploaded file to view its properites to get URL.
17. Pasted URL into another window for the result:
<img width="823" height="989" alt="image" src="https://github.com/user-attachments/assets/383453bb-3e6e-4f29-a986-b456221cd1e0" />

### Summary

All in all pretty straightforward, uploading files is fairly simple, finding URL also, creation is nothing complex. Always make sure to set access level to what you need.
