# Lab break fix of Cloud Shell

## The "I was here" log of mistakes, what went wrong, how i fixed it.

## Timeout problem

**The Fix:** If the session times out, click `Reconnect` button on the screen.

**Verification:** Type `df -h` after reconnecting to make sure your storage drive remounted correctly.

## Script run canceled

**The problem:** Accidentally hitting `CTRL + C` to copy text, but it canceled the script

* **The Fix:**
  * You can only use `CTRL +C` to copy if text is **already selected**. If nothing is highlighted, it sends a "kill" command to the terminal.

## Uploaded a file but cant find it in CloudDrive

* **The Fix:**
  * By default uploads go to `/home/user`. You have to manually move them to `~/clouddrive` if you want to keep them permanently.

