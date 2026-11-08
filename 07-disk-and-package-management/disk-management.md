Linux Disk Management – Complete Notes
1. Introduction to Disk Management

Disk management in Linux refers to managing storage devices, partitions, filesystems, and disk space.

It helps system administrators and DevOps engineers to:

Check available disk space.
Identify storage devices.
Create and manage partitions.
Format disks with filesystems.
Mount and unmount filesystems.
Monitor disk usage.
Manage swap space.
Troubleshoot disk-related issues.

Real-world scenario:

Suppose a Linux server is running out of disk space because application logs are consuming storage. A DevOps engineer must identify which filesystem is full, locate large files, and free up space safely.

2. Understanding Linux Storage Architecture

Linux storage management follows this general structure:

Physical Disk → Partition → Filesystem → Mount Point → Directory

Example:

Physical disk: /dev/sda
Partition: /dev/sda1
Filesystem: ext4
Mount point: /data
Important storage terms

Disk: A physical or virtual storage device.

Partition: A logical division of a disk.

Filesystem: A structure used by Linux to organize and store files.

Mount Point: A directory where a filesystem is attached and accessed.

Block Device: A device that stores and retrieves data in blocks.
Examples:


/dev/sda

/dev/sdb

/dev/nvme0n1

3. Identify Disks and Storage Devices

3.1 lsblk


lsblk displays block devices such as disks and partitions.


Syntax:


bash
lsblk [options]


Examples:


bash
lsblk
lsblk -f
lsblk -a
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT


Important options:

Option	Description
-f	Display filesystem information
-a	Display all devices
-o	Select specific columns
-p	Display full device paths
-l	Display list format

Example output:

text
NAME        SIZE TYPE MOUNTPOINTS
sda          50G disk
├─sda1        1G part /boot
└─sda2       49G part /
sdb         100G disk
└─sdb1      100G part /data

3.2 fdisk -l

Displays partition information for disks.

bash
sudo fdisk -l


It shows:

Disk size.
Partition table type.
Partition sizes.
Partition types.
Sector information.
3.3 blkid

Displays block device UUIDs and filesystem types.

bash
sudo blkid


Example:

text
/dev/sda1: UUID="abcd-1234" TYPE="ext4"

3.4 ls -l /dev/

Displays device files.

bash
ls -l /dev/sd*
ls -l /dev/nvme*


Common device naming:

Device	Meaning
/dev/sda	First SATA/SCSI-style disk
/dev/sdb	Second SATA/SCSI-style disk
/dev/sda1	First partition of sda
/dev/nvme0n1	First NVMe namespace
/dev/nvme0n1p1	First partition of NVMe namespace
4. Check Disk Space
4.1 df – Disk Free

The df command displays filesystem disk space usage.

Syntax:

bash
df [options] [filesystem]


Examples:

bash
df
df -h
df -T
df -i
df -Th


Important options:

Option	Description
-h	Human-readable sizes
-T	Display filesystem type
-i	Display inode usage
-a	Include pseudo and empty filesystems
-k	Display sizes in KB

Example:

bash
df -h


Sample output:

text
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        50G   35G   15G  70% /
/dev/sdb1       100G   60G   40G  60% /data


Explanation:

Size: Total filesystem capacity.
Used: Space already occupied.
Avail: Available space.
Use%: Percentage of used storage.
Mounted on: Directory where filesystem is mounted.

Interview point: If a server reports 100% disk usage, start with df -h to identify the full filesystem.

4.2 du – Disk Usage

The du command displays directory and file space usage.

Syntax:

bash
du [options] [path]


Examples:

bash
du -sh /var
du -sh /var/log
du -h /home
du -ah /var/log
du -sh *


Important options:

Option	Description
-h	Human-readable output
-s	Summary
-a	Include files
-c	Display total
--max-depth=1	Limit directory depth

Find the largest directories:

bash
sudo du -h --max-depth=1 /var | sort -hr


Find the largest files:

bash
sudo find /var -type f -exec du -h {} + | sort -hr | head -10


Difference between df and du:

df	du
Displays filesystem usage	Displays file and directory usage
Checks mounted filesystem space	Checks storage occupied by files
Useful for identifying full partitions	Useful for finding large directories
5. Linux Disk Partitioning

Partitioning divides a physical disk into logical sections.

Common partition table types:

MBR (Master Boot Record)
Older partition table format.
Commonly limited to approximately 2 TiB disks with 512-byte logical sectors.
Supports up to four primary partition entries.
Extended partitions allow additional logical partitions.
GPT (GUID Partition Table)
Modern partition table format.
Supports large disks.
Supports many partitions, depending on the operating system and tools.
Stores backup partition table information.

GPT is commonly used in modern Linux systems.

5.1 fdisk

fdisk is a command-line disk partitioning utility.

Syntax:

bash
sudo fdisk /dev/sdX


Example:

bash
sudo fdisk /dev/sdb


Common interactive commands:

Command	Description
m	Display help
p	Print partition table
n	Create new partition
d	Delete partition
t	Change partition type
w	Write changes and exit
q	Quit without saving changes

Important: Partitioning and deleting partitions can destroy data. Always verify the target device and back up important information.

5.2 parted

parted is another partition management utility.

Examples:

bash
sudo parted -l
sudo parted /dev/sdb


Inside parted:

text
print
mklabel gpt
mkpart primary ext4 1MiB 100%
print
quit


The above commands illustrate creating a GPT partition table and one partition occupying almost the entire disk.

Warning: mklabel replaces the existing partition table. Never run it on a disk containing data you need to preserve.

6. Filesystems in Linux

A filesystem determines how data is stored and organized.

Common Linux filesystems:

Filesystem	Description
ext4	Widely used general-purpose Linux filesystem
XFS	High-performance journaling filesystem
Btrfs	Supports snapshots and advanced storage features
ext3	Older journaling filesystem
vfat	Commonly used for EFI system partitions and removable media
swap	Used for virtual memory
6.1 ext4
Common default filesystem in many Linux distributions.
Supports journaling.
Suitable for general-purpose storage.
6.2 XFS
Common in RHEL-family distributions.
Designed for high-performance workloads.
Supports online filesystem growth.
Does not support shrinking through its normal filesystem tools.
6.3 Btrfs
Supports snapshots.
Supports checksums.
Supports compression.
Provides advanced storage management features.
7. Formatting a Partition

Formatting creates a filesystem on a partition.

7.1 Create an ext4 filesystem

Syntax:

bash
sudo mkfs.ext4 /dev/sdX1


Example:

bash
sudo mkfs.ext4 /dev/sdb1

7.2 Create an XFS filesystem
bash
sudo mkfs.xfs /dev/sdb1

7.3 Check filesystem type
bash
lsblk -f
sudo blkid


Important: Formatting a partition destroys the existing filesystem data. Never format a partition without verifying its identity and contents.

8. Mounting and Unmounting Disks

Mounting makes a filesystem accessible through a directory.

8.1 Create a mount point
bash
sudo mkdir -p /data

8.2 Mount a partition

Syntax:

bash
sudo mount DEVICE MOUNT_POINT


Example:

bash
sudo mount /dev/sdb1 /data


Verify:

bash
df -h
lsblk
findmnt

8.3 Unmount a partition

Syntax:

bash
sudo umount MOUNT_POINT


Example:

bash
sudo umount /data


Note: The command is umount, not unmount.

8.4 Common mount options
Option	Description
defaults	Use default mount options
ro	Mount read-only
rw	Mount read-write
noexec	Prevent execution of binaries
nosuid	Ignore set-user-ID and set-group-ID bits
nodev	Do not interpret device files

Example:

bash
sudo mount -o ro /dev/sdb1 /data


This mounts the filesystem read-only.

8.5 Find mounted filesystems
bash
mount
findmnt
findmnt /data

9. Permanent Mounting Using /etc/fstab

/etc/fstab is used to configure filesystems that should be mounted automatically.

File location:

bash
/etc/fstab


Typical format:

text
UUID=<filesystem-uuid> /data ext4 defaults 0 2


Fields:

Field	Meaning
UUID	Unique filesystem identifier
Mount point	Directory where filesystem is mounted
Filesystem type	ext4, XFS, etc.
Mount options	defaults, ro, rw, etc.
Dump	Backup utility field
Pass	Filesystem check order
Why use UUID instead of /dev/sdX?

Device names such as /dev/sdb1 may change depending on device discovery order.

UUIDs provide a more stable way to identify filesystems.

Find UUID
bash
sudo blkid

Edit fstab
bash
sudo nano /etc/fstab


Add the appropriate UUID-based entry.

Test fstab
bash
sudo findmnt --verify
sudo mount -a


If the command completes without errors, the configuration has passed these basic checks.

Important: Incorrect fstab entries can cause boot problems. Back up the file before editing:

bash
sudo cp /etc/fstab /etc/fstab.backup

10. Swap Space Management

Swap is disk space used as virtual memory when RAM is insufficient or when the kernel needs to move inactive memory pages out of RAM.

Swap can be configured using:

Swap partitions.
Swap files.
Check swap usage
bash
free -h
swapon --show
cat /proc/swaps

Check memory
bash
free -m
free -h

Enable an existing swap area
bash
sudo swapon /dev/sdX2

Disable swap
bash
sudo swapoff /dev/sdX2

Swap file concept

A swap file is a regular file configured for use as swap space.

Common commands involved in swap file administration:

bash
sudo fallocate
sudo chmod
sudo mkswap
sudo swapon
sudo swapoff


Example of checking swap configuration:

bash
swapon --show


Interview point: Swap is not a replacement for sufficient physical RAM. Excessive swapping can significantly reduce system performance.

11. Inode Management

An inode is a filesystem data structure containing metadata about a file.

It stores information such as:

File permissions.
Owner and group.
File size.
Timestamps.
Data block pointers.

The filename itself is stored in the directory entry, not inside the inode.

Check inode usage
bash
df -i


Example:

text
Filesystem      Inodes   IUsed   IFree IUse%
/dev/sda2      5000000  4900000 100000  98%


A filesystem can run out of inodes even when disk space is available.

Real-world scenario:

An application creates millions of small temporary files. The disk still has free gigabytes, but applications cannot create additional files because the filesystem has exhausted its inodes.

Troubleshooting:

bash
df -i
sudo du -sh /var/*

12. LVM – Logical Volume Management

LVM provides flexible storage management in Linux.

It allows administrators to manage storage using logical volumes instead of depending only on fixed disk partitions.

LVM architecture

Physical Volume (PV) → Volume Group (VG) → Logical Volume (LV) → Filesystem

Components

Physical Volume (PV):

A disk or partition initialized for LVM.

Volume Group (VG):

A storage pool created from one or more physical volumes.

Logical Volume (LV):

A virtual storage device allocated from a volume group.

Important LVM commands
Command	Description
pvcreate	Initialize a physical volume
pvdisplay	Display physical volume details
pvs	Summarize physical volumes
vgcreate	Create a volume group
vgdisplay	Display volume group details
vgs	Summarize volume groups
lvcreate	Create a logical volume
lvdisplay	Display logical volume details
lvs	Summarize logical volumes
lvextend	Extend a logical volume
lvreduce	Reduce a logical volume
vgextend	Add a physical volume to a volume group
LVM example workflow

The following is an illustrative workflow for a new, empty disk. Replace device names only after verifying the actual disk.

bash
sudo pvcreate /dev/sdb


Create a volume group:

bash
sudo vgcreate vg_data /dev/sdb


Create a 10 GB logical volume:

bash
sudo lvcreate -L 10G -n lv_storage vg_data


Create an ext4 filesystem:

bash
sudo mkfs.ext4 /dev/vg_data/lv_storage


Create a mount point:

bash
sudo mkdir -p /data


Mount:

bash
sudo mount /dev/vg_data/lv_storage /data


Verify:

bash
sudo pvs
sudo vgs
sudo lvs
df -h

Extend a logical volume

Example:

bash
sudo lvextend -L +5G /dev/vg_data/lv_storage


This increases the logical volume size by 5 GB.

For an ext4 filesystem, grow the filesystem:

bash
sudo resize2fs /dev/vg_data/lv_storage


For an XFS filesystem, grow it using the mounted directory:

bash
sudo xfs_growfs /data


Some LVM systems support combining logical-volume and filesystem growth:

bash
sudo lvextend -r -L +5G /dev/vg_data/lv_storage

Important LVM warning

Shrinking filesystems is more dangerous than extending them.

XFS cannot be shrunk using its standard filesystem tools.
ext4 must be unmounted and checked before shrinking.
Incorrect shrinking operations can destroy data.
13. Disk Monitoring and Troubleshooting

Important commands:

bash
df -h
df -i
du -sh /var/*
lsblk
findmnt
iostat
iotop

iostat

Displays CPU and disk I/O statistics.

Install on Debian/Ubuntu:

bash
sudo apt install sysstat


Install on RHEL-family distributions:

bash
sudo dnf install sysstat


Run:

bash
iostat -xz 1

iotop

Displays disk I/O usage by processes.

Installation:

bash
sudo apt install iotop


On RHEL-family systems:

bash
sudo dnf install iotop


Run:

bash
sudo iotop


Availability and package names can differ by distribution and release.

Check disk I/O statistics
bash
cat /proc/diskstats

Check kernel disk messages
bash
dmesg | grep -iE 'disk|error|I/O'

14. Common Disk Problems and Solutions
Problem 1: Disk usage is 100%

Check:

bash
df -h


Find large directories:

bash
sudo du -h --max-depth=1 /var | sort -hr


Possible causes:

Large application logs.
Temporary files.
Backup files.
Old application data.
Problem 2: No space left on device

Check:

bash
df -h
df -i


Possible causes:

Filesystem is full.
Inodes are exhausted.
Application cannot write to the target filesystem.
Problem 3: Filesystem is mounted read-only

Check:

bash
findmnt
dmesg | tail -50


Possible causes:

Filesystem errors.
Storage hardware problems.
Explicit read-only mount options.

Do not immediately remount read-write without investigating the underlying issue.

Problem 4: Cannot unmount a disk

Check which processes are using the mount:

bash
sudo fuser -vm /data


Or:

bash
sudo lsof +f -- /data


Close the relevant processes and try again.

Problem 5: Disk space does not decrease after deleting files

A process may still have a deleted file open.

Check:

bash
sudo lsof +L1


Restart or reload the responsible service appropriately to release the open file descriptor.

15. Important Disk Management Commands Cheat Sheet
Command	Purpose
lsblk	List disks and partitions
fdisk -l	Display partition tables
blkid	Display UUID and filesystem information
df -h	Check filesystem disk usage
du -sh	Check directory size
mount	Mount filesystems
umount	Unmount filesystems
findmnt	Display mounted filesystems
mkfs.ext4	Create ext4 filesystem
mkfs.xfs	Create XFS filesystem
parted	Manage disk partitions
free -h	Check memory and swap
swapon --show	Display swap devices
df -i	Check inode usage
pvs	Display LVM physical volumes
vgs	Display LVM volume groups
lvs	Display LVM logical volumes
iostat	Monitor disk I/O
iotop	Monitor process disk I/O
16. Practical DevOps Scenarios
Scenario 1: Production server disk is full

Steps:

Check filesystem usage.
bash
df -h

Identify the full mount point.
Find large directories.
bash
sudo du -h --max-depth=1 /var | sort -hr

Inspect logs and application data.
Identify unnecessary files.
Clean up only after verifying retention requirements and backups.
Verify available space.
bash
df -h

Scenario 2: New disk is attached to a Linux server

Tasks:

Identify the new disk.
bash
lsblk

Inspect its existing partitions and filesystem.
bash
sudo fdisk -l
sudo blkid

Plan partitioning and filesystem creation.
Create a filesystem only if the disk is confirmed to be new or disposable.
Create a mount point.
Mount the filesystem.
Configure /etc/fstab using UUID.
Test the configuration.
Scenario 3: Application cannot create files

Check:

bash
df -h
df -i
ls -ld /data


Investigate:

Disk capacity.
Inode availability.
Directory permissions.
Read-only mount status.
Scenario 4: Increase application storage

If the server uses LVM:

Identify free space in the volume group.
bash
sudo vgs

Extend the logical volume.
Grow the filesystem using the correct filesystem-specific command.
Verify.
bash
df -h
sudo lvs

17. Hands-on Practice Exercises

Practice these commands on a Linux virtual machine.

Display all disks and partitions using lsblk.
Check disk space using df -h.
Check inode usage using df -i.
Find the size of /var/log.
Display filesystem types using lsblk -f.
Find UUIDs using blkid.
Identify mounted filesystems using findmnt.
Check memory and swap usage using free -h.
Identify large directories under /home.
Explain the difference between df and du.

Advanced exercises (use a disposable virtual machine and an empty secondary disk):

Create a new partition.
Format it with ext4.
Mount it under /data.
Configure automatic mounting using UUID in /etc/fstab.
Create an LVM physical volume, volume group, and logical volume.
Extend an LVM logical volume and its filesystem.

Never experiment with partitioning or formatting on your production disk.

18. Linux Disk Management Interview Questions
Beginner Level

1. What is disk management in Linux?

Disk management is the process of managing storage devices, partitions, filesystems, mount points, and available disk space.

2. What is the difference between a disk and a partition?

A disk is a storage device, while a partition is a logical division of that disk.

3. Which command displays disk space usage?

bash
df -h


4. Which command displays directory size?

bash
du -sh /path


5. What is the difference between df and du?

df displays filesystem usage, while du displays the space consumed by files and directories.

6. What is mounting?

Mounting attaches a filesystem to a directory so that its contents can be accessed.

7. What is the purpose of /etc/fstab?

It stores filesystem mount configuration, including filesystems intended for automatic mounting.

Intermediate Level

8. What is the difference between MBR and GPT?

MBR is an older partition table format with a roughly 2 TiB limit under common 512-byte sector assumptions and four primary partition entries. GPT is a modern format that supports larger disks and many partitions.

9. What is a UUID?

UUID is a unique identifier associated with a filesystem or storage object. It can be used to identify filesystems in /etc/fstab.

10. What happens when a filesystem reaches 100% usage?

Applications may fail to create or write files. Services may malfunction, and system operations may be affected.

11. Can a filesystem run out of inodes while disk space is available?

Yes. A filesystem can exhaust its inode capacity because of a very large number of small files.

12. What is swap space?

Swap is storage space used as virtual memory to hold memory pages moved out of RAM.

13. What is the difference between ext4 and XFS?

Both are journaling filesystems. ext4 supports shrinking while unmounted using the appropriate tools. XFS supports online growth but not shrinking through its standard tools.

Advanced Level

14. What is LVM?

Logical Volume Management provides flexible storage allocation using physical volumes, volume groups, and logical volumes.

15. Explain PV, VG, and LV.

PV: Physical Volume.
VG: Volume Group.
LV: Logical Volume.

16. How do you extend an LVM logical volume?

Use lvextend to increase its size, then grow the filesystem using resize2fs for ext4 or xfs_growfs for XFS.

17. How do you troubleshoot a disk-full issue?

Start with df -h, check inode usage with df -i, locate large directories using du, and inspect deleted-but-open files using lsof +L1.

18. Why is UUID preferred over device names in fstab?

Device names may change during device discovery, whereas UUIDs provide a more stable filesystem identifier.

19. What is the difference between mounting and formatting?

Formatting creates a filesystem. Mounting makes an existing filesystem accessible through a directory.

20. Why might deleted files not free disk space immediately?

A running process may still hold an open file descriptor to a deleted file. The space is released when the last reference is closed.

19. Important Commands to Remember for Interviews
bash
lsblk
lsblk -f
sudo fdisk -l
sudo blkid
df -h
df -i
du -sh /var
findmnt
mount
umount
free -h
swapon --show
sudo pvs
sudo vgs
sudo lvs
iostat -xz 1
sudo lsof +L1

20. Final Revision

Linux disk management is an important DevOps skill because production servers continuously generate application data, logs, temporary files, and backups.

A DevOps engineer should understand:

Disk identification.
Partition management.
Filesystem creation.
Mounting and unmounting.
Permanent mounts using /etc/fstab.
Disk and inode monitoring.
Swap management.
LVM administration.
Disk troubleshooting.
Storage expansion.

Interview preparation tip:

Do not just memorize disk management commands. Practice identifying a full filesystem, locating large files, mounting a new disk, and extending an LVM volume in a disposable Linux virtual machine.

