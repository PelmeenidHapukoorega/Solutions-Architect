# PowerShell commands

This is where i list relevant commands i have used for powershell and explain briefly what each one doess.


* `$PSVersionTable` - Verifies powershell installation and shows current installed version, platform and edition.
  * Running this command results in table output, but its an object. You can use `.` to access specific property such as `PSVersion`
* `$PSVersionTable.PSVersion` - Gives more details about version of PowerShell
* `Get-Verb` - Returns a list of approved verbs categorized by their functional intent. Using these verbs prevents "CommandNotFound" issues and ensures your scripts align with the .NET framework standards.

## Core Cmdlets and what they do

* `Get-Command` - Lists all available cmdlets on the system. Filter the list to quickly find the command you need.
  * If you arent sure of the exact name you can use `*` as the wildcard. This searches the Name property by default. For example `Get-Command *Network*`
* ` Get-Help` - Built-in help system. You can also run an alias help command to invoke `Get-Help` but improve the reading experience by paginating the response.
* `Get-Member` - When you call a command, the response is an object that contains many properties. Run the Get-Member core cmdlet to drill down into that response and learn more about it.

 ## Filtering

 * `Get-Command -Verb Set` - Find all "Set" commands.
 * `Get-Command -Noun *Disk*` - Find all "Disk" commands.
 * `Get-Command *firewall*` - Search by partial name.
 * `Get-Command -Module Az.Websites` - List only Azure Web App tools.
 * `Get-Command -CommandType Cmdlet` - Filter by type (cmdlet vs function).

 ## Flags

 * `-Noun` - flag targets the part of the command name thats related to the noun.
 * `-Verb` - flag targets the part of the command name thats related to the verb. You can combine the `-Noun` flag and the `-Verb` flag to create an even more detailed search query and type.

 * `Get-Command -Noun alias*` - This command searches for all cmdlets whose noun part starts with alias. But it works for anything else that would want to search, just switch `alias` to smt else that you need.
 * `Get-Command -Verb Update -Noun *Network*` - This allows you to narrow the search to specify that the verb part needs to match `Update`, and the noun part needs to match `*Network*`.

