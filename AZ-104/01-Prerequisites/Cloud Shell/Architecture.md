# Architecture of Cloud Shell

## Things needed for Cloud Shell to function properly

Below i have listed below all the DOs and DONTs for cloud shell to function properly.

### What to do

* Remember that each browser tab is a **seperate process**, however they all run on the **same machine (container)**
* Use the **Web Preview** feature if you need to test an app on  a specific port


### What not to do

* Expect files in `/home/user` to survive a **Restart** or a **Timeout**. Only the `clouddrive` folder is safe
* Try to upload **folders** using the drag and drop feature; it only supports individual files.

