# Recover deleted files

### Where do my files go when deleted?

First of all we need to differentiate between deleting from "sending to trash bin".

Sending a file to a trash bin simply puts the file into a special folder that will auto-delete files inside every N days, determined by a "cron job"/"task scheduler".\
When a file is deleted the filesystem will update it's data structure, marking the inode as free in the file system, meaning it's available to be over-written, but until it is done so, the files will still remain in the disk.\


### Data recovering tools

Here enter the data recovery tools, software that analyzes the filesystem structures and searches for traces of deleted files in the free space. They look for information such as file headers, footers or metadata that may still exist in the disk, then they try to reconstruct the remnants to restore the full file.\
They may use techniques like data carving that involves searching for specific file signatures or patterns in the raw data. For example a JPEG file has a distinct header and footer that will make recognizing and locating the rest of the fragments of the file easier.



### Limitations

The effectiveness of file recovery depends on many factors, such as how much of the data has been overwritten, the filesystem format, the circumstances of the file deletion... If a file has been partially overwritten you may be able to recover it but only a part for example.

To prevent a file from being overwritten it's crucial to stop or minimize disk activity the moment you realize you need to recover a file, avoid any type of file creation or modification and if you can turn off the system, boot with a live CD and run a data recovering tool on the partition you want to recover while it is unmounted for maximum effectiveness.



***

### Tools

#### TestDisk

CLI based, multi-platform, my personal choice, may be intimidating at first but once you try it a few times you will see it's super powerful and easy.

{% embed url="https://www.cgsecurity.org/wiki/TestDisk" %}
by CGSecurity
{% endembed %}

#### Recuva by CCleaner

GUI based, Windows, it may be proprietary software and CCleaner hasn't the best of reputations, but free version does the work, super easy to use.&#x20;

{% embed url="https://www.ccleaner.com/recuva" %}
by CCleaner
{% endembed %}

