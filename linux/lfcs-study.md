# LFCS Study

Remember the following two utilities:

man -k {search pattern}
util --help (where util is the program name)


# Hostname (/etc/hostname)
``` sh
hostnamectl set-hostname <name>
```


# Disable Fedora /tmp ramdisk
``` sh
sudo systemctl mask tmp.mount
```


# Show Size of directories:
``` sh
sudo du -shxc --exclude=proc *
sudo du -shxc *
sudo du --max-depth=1 -hx /
```

# Show what a file is for
``` sh
file nameoffile
```

# Processes
``` sh
# == ulimit ========================================================================================
ulimit -a         # Show limits
ulimit -n 1600    # Increase max file descriptors to 
                  # 1600, in the current shell
ulimit -S -n	  # Show soft limit
ulimit -H -n      # Show hard limit

vim /etc/security/limits.conf  # Make these changes permanent

# == ps ============================================================================================
ps -elf           # Show all processes, with Parent ID (PPID). 
ps auxf           # Show all processes, with resource information and ancestry. Additional info:
                  # - VSZ is the process' virtual memory size in KB
                  # - RSS is the non-swapped physical memory a task is using in KB
                  # - STAT describes the state of the process. S - sleeping, R - running. 
                  #   < for high priority (not nice), N for low priority, etc 

ps -o pid,uid,cputime,pmem,command   # The -o flag allows for output formatting

pstree [id]       # Graphical tree of processes

nice -n 5 command [args] # Set Niceness, higher = lower priority
renice +3 PID     # Change niceness for a running program

ipcs              # Show System V IPC applications 
                  # (inter-process communication)
ipcs -p           # Show processes
ps aux | grep -e PID # Show which program is attached to that process
```

### Libraries
``` sh
ldd /path/to/file   # Show library dependancies
echo $LD_LIBRARY_PATH
```

### Process Signals
``` sh
kill -l              # List the signals you can send
kill -9 PID          # Send the SIGKILL signal to the PID
                     # The default is SIGTERM
killall name         # Kill all processes matching name
pkill -9 rafael bash # Sends SIGKILL to all of rafael's
                     # bash processes
```

### Git
``` sh
git log              # Show commits
git diff             # Show changes
```

### rpm
``` sh
rpm --rebuilddb      # Rebuild the package database
rpm -q packagename   # Which version of a package is installed
rpm -qf /bin/bash    # Which package did this file come from
rpm -ql bash         # Which files were installed by this package
rpm -qi bash         # Show information about the package
rpm -qa              # Show all installed packages
rpm -q --whatprovides bash # Show which package provides bash

rpm -Va              # Verify all packages on the system
rpm -V package       # Verify a single package

# The output of the above to commands is:
#
# S - file size differs
# M - file permissions and/or type differs
# 5 - MD5 checksum differs
# D - device major/minor number mismatch
# L - user ownership differs
# G - group ownership differs
# T - modification time differs
# P - capabilities differ
#
# There is no output when everything is okay.

rpm -qil bash

rpm -ivh             # install a package
rpm -e               # erase / uninstall a package
rpm -e --test        # Test if rpm would work, but don't do it
rpm -Fvh *.rpm       # Freshen - upgrade existing packages, but don't
                     # install new ones
```

### rpm2cpio
``` sh
rpm2cpio foo.rpm > foo.cpio  # Copy an rpm to cpio archive, for extraction
rpm -qilp foo.rpm            # List files in an rpm

rpm2cpio foo.rpm | cpio -ivd /dir  # Extract onto the system
# or
rpm2cpio foo.rpm | cpio --extract --make-directories /dir
```

# yum
``` sh
# GUI utility: yumex

yum clean all       # Remove all cached information

# A basic repo file is as follows:

[repo-name]
    name=Description of the repository
    baseurl=http://example.com/path/to/repo
    enabled=1                # 0 = disabled
    gpgcheck=0               # 1 = enabled

yum info package             # Information regarding a package
yum deplist package          # Show dependencies

sudo yum list [installed | updates | available ]
sudo yum grouplist
sudo yum groupinfo
sudo yum groupinstall
sudo yum groupremove
sudo yum provides filename   # Show packages providing the filename

# Package verification requires a yum plugin
# sudo yum install yum-plugin-verify
# Then:
sudo yum verify package

yum localinstall foo.rpm     # Like rpm -ivh, but resolves deps

# Find config files that have been modified during an update:
sudo find /etc -name "*.rpm*"

# Run yum from a text file:
sudo yum shell [text-file]
```

# System Monitoring
These come from the packages **procps** and **sysstat**

``` sh
# Process & Load Monitoring Utilities:
top
uptime
ps
pstree
mpstat         # Multiple processor usage
iostat
sar            # (System Activity Reporter) sCollect and display information about system activity
numastat       # Information about NUMA (Non-Uniform Memory Architecture)
strace

# Memory Monitoring Utilities
free
vmstat         # Virtual memory statistics and block i/o
pmap           # Process memory map

# I/O Monitoring Utilities
iostat         # CPU utilization and I/O statistics

# Network Monitoring Utilities
netstat
iptraf
tcpdump
wireshark

# Changing /proc/sys/kernel parameters:
sudo bash -c 'echo 100000 > threads-max'

ls /sys/class/net     # Show network devices
```

# System Log files
``` sh
sudo tail /var/log/messages    # Find system-wide error messages
dmesg -w                       # Kernel messages. -w is for follow

# Also look at the Gnome log browser
```

# Tunable settings in ```/proc/sys/vm```
admin_reserve_kbytes   Amount of free memory reserved for privileged users
block_dump             Enables block I/O debugging
compact_memory         Turns on or off memory compaction (defragmentation) when configured into the
                       kernel
dirty_background_bytes Dirty memory threshold that triggers writing uncommitted pages to disk
dirty_background_ratio % of total pages as which kernel will start writing dirty data out to disk
dirty_bytes            The mount of dirty memory a process needs to initiate writing on its own
dirty_expire_centisecs When dirty data is old enough to be written out in 100ths of a second
dirty_ratio            % of pages at which a process writing will start writing out dirty data on 
                       its own
dirty_writeback_centisecs  Interval in which periodic writeback daemons wake up to flush. If set to 
                       zero, there is no automatic periodic writeback
drop_caches            Echo 1 to free page cache, 2 to free dentry and inode caches, 3 to free all. 
                       Note only **clean** cached pages are dropped; do **sync** first to flush 
                       dirty pages
extfrag_threshold      Controls when the kernel should compact memory
hugepages_treat_as_movable  Used to toggle how huge pages are treated
hugetlb_shm_group      Sets a group ID that can be used for System V huge pages
laptop_mode            Powersaving on laptops
legacy_va_layout       Use old 2.4 kernel layout for how memory mappings are displayed
lowmem_reserve_ratio   Controls how much memory is reserved for pages that can only be there; ie 
                       pages which can go in high memory instead will do so. Only important on 32-bit systems with high memory
max_map_count          Max number of memory mapped areas a process may have. The default is 64k
min_free_kbytes        Min free memory that must be reserved in each zone
mmap_min_addr          How much address space a user process cannot memory map. Used for security 
                       purposes, to avoid bugs where accidental kernel null dereferences can overwrite the first pages used in an application
nr_hugepages           Minimum size of hugepage pool
nr_pdflush_hugepages   Max size of the hugepage pool = nr_hugepages*nr_overcommit_hugepages
nr_pdflish_threads     Current number of pdflush threats; not writable
oom_dump_tasks         If enabled, dump information produced when oom-killer cuts in
oon_kill_allocating_task  If set, the oom-killer kills the task that triggered the out of memory 
                       situation, rather than trying to select the best one
overcommit_kbytes      One can set either overcommit_ratio or this entry, but not both
overcommit_memory      If 0, kernel estimates how much free memory is left when allocations are 
                       made. If 1, permits all allocations until memory actually does run out. 
                       If 2, prevents any overcommission.
overcommit_ratio       If overcommit_memory = 2 memory commission can reach swap plus this 
                       percentage of RAM
page-cluster           Number of pages that can be written to swap at once, as a power of two. 
                       Default is 3 (which means 8 pages)
panic_on_oom           Enable system to crash on an out of memory situation
percpu_pagelist_fraction  Fraction of pages allocated for each cpu in each zone for hot-pluggable 
                       CPU machines
scan_unevictable_pages If written to, system will scan and try to move pages to try and make them
                       reclaimable
stat_interval          How often vm statistics are updated (default 1 second) by vmstat
swappiness             How aggressively should the kernel swap
user_reserve_kbytes    If overcommit_memory is set to 2 this sets how low the user can draw memory 
                       resources
vfs_cache_pressure     How aggressively the kernel should reclaim memory used for inode and dentry 
                       cache. Default is 100; if 0 this memory is never reclaimed due to memory 
                       pressure.

See also /proc/meminfo for details of memory statistics 


# I/O Monitoring and Tuning
These are tools that you would use to monitor and tune disk I/O:
``` sh
iostat -m              # Show in MB rather than KB
iostat -N              # Show I/O stats by device
iostat -xm             # Extended report (in MB)

iotop -o               # Only show processes actively engaged in I/O

ionice                 # Let's you set scheduling class and priority for a process.
                       # 0 = Default, 1 = Real Time, 2 = Round Robin, 3 = No access unless no other program wants it
                       # ionice only works if using the **CFQ I/O Scheduler**

```

## Schedulers
These are kept in /sys/block/{DEVICE-NAME}/queue/scheduler

CQF          Completely Fair Queue, attempts to anticipate I/O
Deadline     CentOS default
noop         Simple FIFO scheduler

You can check if a disk is rotational in /sys/block/{DEVICE}/queue/rotational. 1 = rotational

You can change the IO scheduler as follows:
``` sh
cat noop > /sys/block/{DEVICE}/queue/scheduler

```

# File Systems and the VFS
Linux, like other OSs, uses a Virtual File System, so that applications interact with that layer to manipulate files.This also allows network file systems to be handled transparently. Every file is associated with an inode, which stores permissions, group/owner, and timestamps.

* Hard links point to an inode
* Soft/Symbolic links point to a file, which has an associated inode

You can see a list of which filesystems a kernel supports under /proc/filesystems. Particular filesystems may only have their code loaded when you try and mount them.

Commands to Know:

``` sh
lsblk
sudo blkid
sudo fdisk -l {/dev/sda}    # List partitions

# Backup an MBR partition
sudo dd if=/dev/sda of=mbrbackup bs=512 count=1

# And restore that MBR partition
sudo dd if=mbrbackup of=/dev/sda bs=512 count=1

# For GPT partitions, use sgdisk
sudo sgdisk --backup=sda_backup /dev/sda

# And restore
sudo sgdisk --load-backup=sda_backup /dev/sda

# Attempt to read new partitions without rebooting
sudo partprobe -s

# You can see what partitions the system is aware of at any time by doing:
cat /proc/partitions
```


# Use & Maintenance of filesystems
Extended attributes cinlude four namespaces: user, trusted, security (SELinux), and system (ACLs). These are viewable with:

``` sh
lsattr filename
chattr filename
```

Flags may be set for files:
* i - Immutable. Cannot be modified, deleted or renamed even by root. No hardlink can be created to it.
* a - Append-only. Can only be oppened in append mode for writing.
* d - No-dump. It is ignored when the *dump* program is run. This is useful for swap and cache files that don't need to be backed up.
* A - No atime update. This file will not update it's accessed time when accessed but otherwise not modified. This can increase performance on some systems, since it reduces disk I/O.


You can fix filesystem errors with fsck and it's sub-programs:

``` sh
fsck.ext4 /dev/sda1             # Requires the filesystem to be unmounted
dumpe2fs /dev/sda1              # Shows filesystem information
tune2fs /dev/sda1               # Shows and changes filesystem parameters
tune2fs -i 180 /dev/sda1        # Change interval between checks to 180 days

sudo mount -t ext /dev/sda1 /mnt
sudo umount /mnt

# You can also use labels and UUIDs to specify mounts
sudo mount UUID=2462272-3453-etc /mnt
sudo mount LABEL=home /home

# You can also re-mount with different options
sudo mount -o remount,ro /mnt

# To check which users/processes might be using files:
lsof | grep {filename}      # List open files
fuser -v /mnt               # Show who is using what file in /mnt

# Network File Shares
# Can be mounted with the following:
sudo mount -t [nfs|cifs|afs] myserver.com:/dir /mnt/dir

# The system may try and mount before the network is up, which is where the netdev and noauto mount options
# come in. It can also be solved by using autofs (Now deprecated) or automount.
```

# Automatic File System Mounting
``` sh
# To automount a file system in systemd, add an appropriate line to /etc/fstab:
LABEL=Sam128 /SAM ext4 noauto,x-systemd.automount,x-systemd.device-timeout=1,x-systemd.idle-timeout=30 0 0

# The mount options are:
# * noauto                            # Don't mount at boot
# * x-systemd.automount               # Use systemd automount
# * x-systemd.device-timeout=10       # (seconds till timeout)
# * x-systemd.idle-timeout=30         # (seconds till idle causes unmount)

# Once that is done, you need to run:

sudo systemctl daemon-reload
sudo systemctl restart local-fs.target
```

# File System Features: Swap, Quotas, Usage

``` sh
# Swap commands are:
mkswap  /dev/sda2
swapon  /dev/sda2
swapoff /dev/sda2

# Quota commands:
# These rely on the existance of aquota.user and aquota.group files in the root directory of the filesystem mountded with the quota options (usrquota and/or grpquota)

quotacheck -ua                         # Generate quota files for users
quotacheck -ga                         # Generate quota files for groups
quotaon                                # Enables quota accounting
quotaoff                               # Disables quota accounting
edquota                                # Edits user or group quotas
quota                                  # Returns the current user quota
quota -g                               # Returns the current group quota
```


# EXT4
Block-size usually defaults to 4096 bytes. The number of inodes on the filesystem can be tuned to save disk space. A feature called **inode reservation** consists of reserving several inodes when a directory is created, expecting them to be used.

* **multiblock allocation** allocates space all at once, instead of one block at a time. Delayed allocation can also increase performance
* **pre-allocate disk space** reserve disk space for a file
* **allocate-on-flush** a performance technique which delays block allocation until data is written to disk
* **fast fsck**
* **checksums for the journal** improves reliability

# XFS & BTRFS

XFS eats CPU time, but it aligns with underlying RAID and LVM structures. It also journals quota information, making it more performant if they are used. Enlarging, Defragmenting, Dumping and Restoring can be done while the file system is online. It does not support snapshots. Backup and Restore can be done with:

``` sh
xfsdump
xfsrestore
```

BTRFS allows snapshots to be done rapidly, and makes extensive use of Copy on Write techniques.



# Encrypting Disks
LUKS is managed using cryptsetup. You can see what ciphers are supported in /proc/crypto

``` sh
yum install cryptsetup-luks

cryptsetup luksFormat --cipher aes /dev/sda1    # Format a volume
cryptsefup luksOpen /dev/sda1 home              # Create a mapping
ls /dev/mapper/home                             # List the mapping
cryptsetup -v status home                       # See the status of the mapping
cryptsetup luksDump /dev/sda1                   # Dump LUKS headers

# You need to create a filesystem on this partition, and mount it. To unmount:

umount /home                                    # Unmount
cryptsetup luksClose home                       # Secure data

# Mounting at Boot
# In /etc/fstab:
/dev/mapper/SECRET /mnt ext4 defaults 0 0

# and in /etc/crypttab:
SECRET /dev/sda1  # See man crypttab for more options
```


# LVM
LVM impacts performance. See man lvm

``` sh
pvcreate              # Converts a partition to a physical volume
pvdisplay             # Shows physical volumes being used
pvmove                # Moves the data from one physical volume within the volume group to others
pvremove              # Remove a partition from a physical volume

vgcreate              # Create volume groups
vgextend              # Adds to volume groups
vgreduce              # Shrinks volume groups
vgdisplay             # Show volume groups

lvcreate              # Create logical volumes, which then need to be formatted and mounted
lvdisplay             # Show available logical volumes
lvresize -r -L 20 GB /dev/vg/mylvm   # Resize a logical volume
lvreduce -r -L -9 GB /dev/vg/mylvm   # Shrink a volume


# So an example sequence would be:
pvcreate /dev/sdb1
pvcreate /dev/sdc1
vgcreate -s 16M vg /dev/sdb1
vgextend vg /dev/sdc1
lvcreate -L 50G -n mylvm vg
mkfs.ext4 /dev/vg/mylvm
mkdir /mylvm
mount /dev/vg/mylvm /mylvm

# then add to /etc/fstab:
/dev/vg/mylvm /mylvm ext4 defaults 0 0

# To grow a logical volume with an ext4 filesystem
lvextend -L +500M /dev/vg/mylvm
resize2fs /dev/vg/mylvm              # Or just use -r in the previous command


# Snapshot
lvcreate -l 128 -s -n mysnap /dev/vg/mylvm

```


# RAID
You can see status in /proc/mdstat. Setting up a RAID array:
``` sh
# Create partitions of the RAID type on two+ disks
fdisk /dev/sdb
fdisk /dev/sdc

# Then set up the array, format it, add to the configuration and mount it:
mdadm --create /dev/md0 --level=1 --raid-disks=2 /dev/sdb1 /dev/sdc1  # use -x 1 to configure a hot spare
mkfs.ext4 /dev/md0
mdadm --detail --scan >> /etc/mdadm.conf
mkdir /mnt/raid
mount /dev/md0 /mnt/raid

# And add in /etc/fstab:
/dev/md0 /myraid ext4 defaults 0 2

# You can get email notifications by adding the following to /etc/mdadm.conf:
MAILADDR rafael.hart9@gmail.com   # then:
systemctl start mdmonitor
systemctl enable mdmonitor

# You can monitor status with:
mdadm --detail /dev/md0
cat /proc/mdstat

# Remove or add individual drives with:
mdadm --remove /dev/md0 /dev/sdb2
mdadm --add /dev/md0 /dev/sde2
```

# Kernel Services and Configuration

``` sh
# You can see the command line the kernel booted with in:
cat /proc/cmdline

# Info on boot parameters is in man bootparam. You can see and alter current parameters with:
sysctl -a   # Show all
sysctl net.ipv4.ip_forward=1 # Modify a setting

# Settings need to be added into /etc/sysctl.conf to be made permanent. Systemd is migrating this to /usr/lib/sysctl.d/00-system.conf. 
# This file can be processed immediately with:
sysctl -p
```

# Kernel Modules
``` sh
modinfo module          # Info about a module
modprobe module params  # Load modules using a pre-built module database with dependency and location information
modprobe -r module      # Unload modules as above
insmod /path/to/module  # Insert a module directly
rmmod /path/to/module   # Remove a module directly
lsmod                   # Show currently loaded modules
depmod                  # Rebuild module dependency database; needed by modprobe and modinfo

# Modules are normally stored under
/lib/modules/<kernel version>

# Boot time parameters and Blacklist are under:
/etc/modprobe.d/
```


# Devices & Udev
``` sh
# Udev is handled by:
systemctl status systemd-udevd

# Udev config and rules
/etc/udev/udev.conf
/etc/udev/rules.d/
```

# KVM
``` sh
# The following tools are provided by libvirt, which supports multiple hypervisors
virt-manager
virt-viewer
virt-install
virsh

# Convert disk files between formats:
qemu-img convert -O vdi myvm.qcow2 myvm.vdi

# Get a list of formats supported
qemu-img --help | grep formats:

```

# Containers
``` sh 
# Look up docker commands with the following:
man docker
man docker-search
man docker-pull
man docker-create
man docker-run

```

# User Management
Users are all recorded in /etc/passwd. If an account is listed as 'not available', check that it has a shell and a password in /etc/shadow. The /sbin/nologin shell returns the contents of /etc/nologin.txt.
``` sh
# Create a new user
useradd -m <username>

# Remove a user
userdel -r <username>

# Modify the user with 'usermod'. The following adds the user to a group:
usermod -aG <group> <username>

# This locks/unlocks a user account
sudo usermod -L <username> # lock
sudo usermod -U <username> # unlock

# Accounts can also be set with password aging
sudo chage -m <mindays> -M <maxdays> -d <lastday> -I <inactive> -E <expiredate> -W <warndays> <username>
sudo chage -d 0 <username> # Must change on next login
sudo chage -l <username>   # List current chage attributes

# Restricted shells - user can't cd, redirect input, or define SHELL / ENV / PATH
bash -r # the user mustn't have u+wx rights on /home, since the shell executes $HOME/.bash_profile

# Restricted accounts - uses restricted shell, limits access times, locations, system resources, available applications
# since parameters can't be passed in /etc/passwd, this can be done with a symlink.
# Ensure user doesn't have system directories in their PATH variable, or they'll be able to execute an unrestricted shell.

# Root login is only permitted through the devices in /etc/securetty

# Setup & run vnc
sudo yum install tigervnc tigervnc-server
vncserver
# You may need to kill the color daemon with
sudo systemctl stop colord
```

# Group Management
Group passwords can be set if /etc/gshadow exists. A user's primary GID should correspond with their own group, and will be used to set the group on any file they create.
``` sh
groupadd -r -g <gid> <groupname>
groupdel <groupname>
groupmod -g 101 blah

chgrp <group> <file> # Change a file's group

```

# Umask & ACLs
The default permissions given when creating a file are read/write for the owner, group and world (0666), and for the a directory it's 0777. This can be changed by setting the umask

``` sh
umask 0022          # don't give group or world execute access
umask u=r,g=w,o=rw  # give user read, group write and world read write access

# ACLs
getfacl <file/directory> # show current acls
setfacl -m u:rafael:rw <file>  # Add an ACL
setfacl -x u:rafael    <file>  # Remove an ACL

```

# Plugable Authentication Modules (PAM)
PAM-Aware applications keep their configuration files in /etc/pam.d. When a user involkes a PAM-aware application such as ssh, the application calls libpam, then checks /etc/pam.d - this deliniates which PAM modules to invoke, including system-auth.

The config files follow the syntax:
<type> <control> <module-path> <module-arguments>

See man pam.d for more.

## type
* auth - instructs the application to prompt the user for identification (user/pass). May set credentials and grant priviledges
* account - checks on aspects of the user's account, such as password aging, access control, etc
* password - responsible for updating the user authentication token, usually a password
* session - used to provide functions before and after the session is established (such as setting up environement, logging ,etc)

## control
* required - must return success for the service to be granted. If part of a stack, all other modules are still executed. The application is not told which module or modules failed
* requisite - same as required, except a failure in any module terminates the stack and a return status is sent to the application
* optional - the module is not required. If it is the only module, then its return status to the application may cause failure
* sufficient - if this module succeeds then no subsecuqent modules in the stack are executed. If it fails, then it doesn't necessarily cause the stack to fail, unless it is the only one in the stack

## module-path
Gives the file name of the library to be found in /lib*/security

## module-arguments
Can be given to modify the PAM module's behaviour.

# LDAP Authentication
LDAP uses PAM and system-config-authentication or authconfig-tui. You need to specify the server, search base DN and TLS. ALso require is openldap-clients, pam ldap and nss-pam-ldapd.

When you configure a system for LDAP authentication, 5 files are changed:

* /etc/openldap/ldap.conf
* /etc/pam_ldap.conf
* /etc/nslcd.conf
* /etc/sssd/sssd.conf
* /etc/nsswitch.conf


# Network Addresses
``` sh
ip                     # Used to configure, control and query interface parameters and control devices, routing, etc.
ip a                   # Show IP addresses & configured devices
ip -s link show eth0   # Show detailed information on a device
ip link set eth0 down  # Bring down the ethernet connection

# Persistant changes need to be made in Network Manager
nmtui

route -n               # Show routing table

# Set a default route by putting the following in /etc/sysconfig/network
GATEWAY=x.x.x.x

mtr <address>          # Ping & Traceroute combo
```

# Firewalls
``` sh


```

