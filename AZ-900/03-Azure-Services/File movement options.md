# Identify Azure file movement options

## Table of contents
* [AzCopy](#AzCopy)
* [Azure storage explorer](#azure-storage-explorer)
* [Azure file sync](#azure-file-sync)

## My interpretations

Azure has tools designed to help move or interact with individual files or small file groups, this is called file movement options.

## AzCopy

Its a command line utility that you can use to copy blobs or files to or from your storage account.

### Why does it exist?

To provide high performance, reliable and scriptable command line utility for moving data into, out of and between Azure storage service.

### What does it do?

It fills the need for a dedicated tool that can handle large scale, automated and nuanced data transfer operations better than basic file copy methods.

### How does it work?

You give it instructions by typing AzCopy command telling it the source and the destination. AzCopy breaks it up into smaller managable pieces, then it starts uploading or downloading the pieces at the same time. AzCopy writes down a list of every file and piece it succesfully transfers in a journal file, if the connection drops AzCopy stops. When you run the command again it checks the journal list and starts from the first piece that it failed aka resumes where it left off.

### Summary

Think of AzCopy as a smart copier that uses all your computers power and remembers exactly where it left off.

## Azure storage explorer

Its a standalone app that provides graphical interface to manage files and blobs in your Azure storage account.

### Why does it exist?

To provide a graphical user interface aka GUI for easily managing contents of Azure storage accounts across different OS systems. 

### What does it do?

It solves the problem of needing to use complex command line tools or navigate the web portal for every storage task.

### How does it work?

It works by using application installed on your desktop to securely connect to and directly manage your Azure storage accounts.

### Summary

Azure storage explorer is a convenient way to manage your storage accounts and individual files within them.

## Azure file sync

Azure file sync is a tool that lets you centralize your file shares in Azure files and keep the flexibility, performance and compatibility of a windows file server. Its like turning on your windows file server into a miniature content delivery network. In other words it lets you replace your expensive, complex and on prem file storage arrays with a single Azure file share, while still giving your users the local speed they expect from  a windows server.

### Why does it exist?

To bridge the gap between the cloud and on prem infra, allowing organizations to centralize their file shares in the cloud while mainting the experience compared to windows file server.

### What does it do?

It transforms a local windows file server into high performance, intelligent cachee for your file shares that are ultimately centralized in the cloud.

### How does it work?

It works by establishing continuous bidirectional synchronization relationship between a folder on a local windows server and azure file share in the cloud. It uses an intelligent local agent to manage data flow and disk space.

### Summary

In essence Azure file sync is like having your own ATM machine, it keeps the money inside for quick transactions (money in. money out) but the money itself is backed up by security guys who switch the deposit boxes inside.

### Benefits

* Use any protocol thats available on windows server, to access your data locally, including SMB, NFS and FTPS.
* Have as many caches as you need globally.
* Replace a failed local server by installing file sync on a new server in the same data center.
* Configure cloud tiering so the most frequently accessed files are replicated locally, while infrequently accessed files are kept in the cloud until requested.
