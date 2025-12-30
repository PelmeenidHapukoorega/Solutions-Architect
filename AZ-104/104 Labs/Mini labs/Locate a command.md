## Mini lab 1: Locating a command

This mini lab is about how to locate commands and run them to understand more about PowerShell.

1. Ran the command `Get-Command` with the flag `-Noun`
    * `Get-Command -Noun File*`

**Output:**
<img width="879" height="169" alt="image" src="https://github.com/user-attachments/assets/c9633c5d-d900-4598-bce4-58f53c7e8be4" />

2. Wanted to filter further by specifying flags `-Verb` and `-Noun`.
   * `Get-Command -Verb Get -Noun *Virtualmachine*`

**Output:**
<img width="822" height="136" alt="image" src="https://github.com/user-attachments/assets/61de7eca-c63c-4a30-b902-63eda3db966d" />

### Summary

I had always wondered that in order to learn more about different PowerShell commands i would have to scan a lot of forums, which is still somewhat useful mind you but using get commands and specifying them with needed verbs and nouns i can filter out commands i need a lot quicker. This is a good way to filter and find commands you know and want specifically, or if you dont remember the entire command but remember keywords then its a good way to find relevant commands.
