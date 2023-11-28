# Getting a disk ready

### Linux with fdisk

1. Indicate what partition table you are going to use MBR or GPT (recommended GPT)

```bash
sudo fdisk /dev/X                 #Initiates fdisk on selected disk device
sudo fdisk -l /dev/X              #Lists basic info about device 
lsblk                             #To list disks info and mountpoints

#Inside fdisk
o                            #Define MBR partition table
g                            #Defines GPT partition table
m                            #Prints help menu
 
```

{% hint style="info" %}
**MBR (Master Boot Record):** old structure, used with BIOS, legacy mode, for old OS's, used for maximum compatibility. Allows only maximum of 3 primary partitions and 1 extended that can be filled with logical partitions, but only the primaries can be booteable and partitions can be of maximum 2TB.

(Windows 7 32bits and below only function with MBR)

\
**GPT(GUID Partition Table):** new structure, used with UEFI with modern computers. Can hold up to 128 primary partitions with practically unlimited space. Also easier to recover in case of corruption and more secure.

(Windows Vista 64bits and above requieres GPT with UEFI)
{% endhint %}

2. Set partitions&#x20;

```bash
p            #Print the actual partition table
n            #Add a new partition

#Inside new partition creator
+500M        #Sets space of 500MB
+50G         #Sets space of 50GB

v            #Verify partition table
d            #Delete a partition
w            #Write changes

```

3. Assign a filesystem&#x20;

```bash
sudo mkfs -t type -L LabelName /dev/X    
mkfs.fat
mkfs.exfat
mkfs.ntfs
mkfs.ext4 

sudo mkswap -L swapName /dev/X   #Give a partition swap
sudo swapon /dev/X               #Turn on swapfile
swapon --show                    #List swap files, size and location
sudo swapoff /swapfile           #Turn off swapfile

```

Most popular types:

* **FAT32:** compatible with almost everything, optimized for removable storage though has a limitation of 4GB for a single file. `mkfs.fat`
* **exFAT:** ExtendedFAT, evolution of FAT32, optimized for removable storage, practically no limit on file or partition size and full compatible with modern OS's though it may not be compatible with old ones. `mkfs.exfat`
* **NTFS:** designed by Microsoft, used on Windows, but is compatible with Linux thanks to drivers and is also useful for removable storage as it has great compatibility, great optimization and no size limitations like FAT32.
* **EXT4:** the most recent versión of Extended Filesystem, native on Linux systems, best performance.\

* SWAP:  special format file/partition that acts like an extensión of RAM, giving as much space as we want from the storage disk so if the RAM gets full the system doesn't crash and can continue creating new processes.\
  But as it is in the disk connected by SATA or NVME maybe, not the RAM DIMM Module, its way more slow, so its best to avoid reaching that limit.

4. Mount the partition&#x20;

```bash
mkdir /mnt/partitionMountpoint              #Create directory(wherever you want, /mnt recommended by FHS) that will act as mount point  ?Where we will have access to the insides of the partition
sudo mount /dev/X /mnt/partitionMountpoint  #Mount the partition in the directory previously created
sudo umount /mnt/partitionMountpoint        #Unmount the partition from the file system, it will still be on /dev/X

```

#### **/etc/fstab**

Configuration file that specifies what filesystems will be mounted and where at the boot of the system. Takes care of our root filesystem!

{% hint style="danger" %}
It's critical for system booting, so when editing take great care and make sure to have a backup copy! Malfunctioning fstab is no fun.\
`cp /etc/fstab /etc/fstab.bak`
{% endhint %}

<pre class="language-bash"><code class="lang-bash">#Adding a new auto mount point
sudo blkid            #List mounted points info and their UUID (Universal Unique Identifier), way more secure for naming a device in fstab and the standard
UUID=longID /mnt/partitionMountpoint formatType defaults 0 2        #Line you need to add to the fstab  !Be careful
sudo mount -a         #Tries to mount all the filesystems mentioned in the fstab, you can reboot too
<strong> 
</strong><strong>#Example: Mounts ntfs filesystem on /mnt/MoviesHDD with ReadOnly permissions
</strong><strong>UUID=332fd7eb-a8be-431d-923b-bc6799628e78 /mnt/MoviesHDD ntfs defaults,ro 0 2      
</strong></code></pre>

{% hint style="info" %}
**Options (fourth field) \[standard defaults]:** defaults parameter includes standard configuration options for easy use of the filesystem, like:

* **rw:** Being able to read-write
* **dev:** Allows to use devices
* **auto:** Allows auto-mount on system start
* **nouser:** Prohibits normal users to mount or dismount the filesystem
* **exec:** Allows to execute binaries in the filesystem\


**Dump (fifth field) \[standard 0]:** not commonly used today, usually skipped (0), if "1" the filesystem is included in the backup process\
\
**Pass (sixth field) \[Others standard 2) \[fsck order]:** order in which filesystem checks are done at boot time, **root filesystem should have 1**, then it after it finishes it goes to 2 for less important filesystems, if 0 the filesystem is not checked (not recommended!)
{% endhint %}





### Windows with diskpart



