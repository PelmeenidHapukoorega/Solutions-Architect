# Architecture of Cloud Shell

## Things needed for Cloud Shell to function properly

1. **Rule:** Must have an **Azure Storage Account** and **Azure File Share** (CloudDrive) to persist data.
  * Files on CloudDrive may be persisted but you need to start a new session to access Cloud Shell environment.

What this means: It means that you can upload scripts and code to built in Drive within CloudShell which then allows you to open another session from a different device and still access the same file.

2. **Rule (Timeout):** Sessions terminate after 20 mins of inactivity.
  * Unsaved work that was created outside of `clouddrive` folder are terminated permanently.

 **DO:** Save all scripts and logs in the `~/clouddrive` directory. Only place where data survives timeout.

 **DONT:** Run long scripts (Like 30 min migration) directly in the shell wihtout a keep alive activity.

3. **Rule (Isolation):** Files saved outside of `CloudDrive` folder are temporary and will be deleted when the session ends.

4. **Rule (Access):** Accessible via `shell.azure.com` Azure portal or Azure Mobile app.
