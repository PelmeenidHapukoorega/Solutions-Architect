## Mini lab: Using Azure command line interface

1. Made Azure account
2. Opened up terminal
    * Tried using commandline to switch between Bash and Powershell by using commands `BASH` and `PWSH`
3. Switched to powerhshell using command `PWSH`
4. Checked AZ version by entering `az version`
5. Entered back into BASH by using, well... `BASH` to test get-date command
6. Was met with the error `bash: Get-date: command not found` because `Get-date` is a powershell specific command.
   Used `Date` instead which worked.
7. Ran an update on powershell `az upgrade`
8. Tried another way to interact with terminal by entering Azure CLI interactive mode with `az interactive`
9. Asked if i want to send telemetery data, responded with `yes`. Portal intialized.
   * In interactive mode the commands work without having to use `az` at the front.
   * Used commands `upgrade` and `version` without the az at the front. 

### Summary

Familiarised myself with Azure interface and learned that i can switch a lot quicker between terminals by using `PWSH` and `BASH`
as needed to. Additionally az interactive mode seems to read commands faster and is more comfortable to use since I didnt need to
use the annoying `az` at the front all the time, felt more efficient.
