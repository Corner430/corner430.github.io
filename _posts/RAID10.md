---
title: RAID10
date: 2023-04-03 18:28:37
tags:
   - Linux
   - RAID
declare: true
---
##### 1. RAID
- RAID 0 acceleration without backup
- RAID 1 backups are not accelerated
- RAID 5 parity, accelerated backup for low cost
- RAID 10 RAID 0 + RAID 1 RAID1 is formed first, backup is not accelerated, and then RAID0 is formed, acceleration is not backed up. Unless all hard disks in one group fail. The utilization rate is 50%. high cost
<!--more-->
##### 2. virtual four parallel disks

##### 3. `/dev`check disk

##### 4. make an array card

`sudo mdadm -Cv /dev/md0 -a yes -n 4 -l 10 /dev/sdb /dev/sdc /dev/sdd /dev/sde`
- The mdadm command is used to manage the software RAID hard disk array in the Linux system
- The -C parameter represents creating a RAID array card
- The -v parameter displays the creation process, and the appended /dev/md0 is the name of the RAID disk array after creation
- -a yes represents automatic creation of device files
- -n 4 parameters represent using 4 fast hard disks for deployment
- -l 10 specifies the RAID level as RAID 10 scheme
- The last is the name of the 4 hard disk devices

##### 5. format
`mkfs.ext4 /dev/md0`

##### 6. mount
```shell
mkdir /RAID
mount /dev/md0  /RAID
df -h
```

回显
```shell
Filesystem                        Size  Used Avail Use% Mounted on
/dev/mapper/rhel_linuxprobe-root   18G  2.9G   15G  17% /
devtmpfs                          905M     0  905M   0% /dev
tmpfs                             914M   92K  914M   1% /dev/shm
tmpfs                             914M  8.8M  905M   1% /run
tmpfs                             914M     0  914M   0% /sys/fs/cgroup
/dev/sda1                         497M  119M  379M  24% /boot
/dev/md0                           40G   49M   38G   1% /RAID
```

##### 7. Persistence
`/etc/fstab写入'/dev/md0                     /RAID                       ext4    defaults        0 0'`

##### 8. check status
` mdadm -D /dev/md0`
回显
mdadm: must be super-user to perform this action
```shell
[corner@linuxprobe ~]$ sudo mdadm -D /dev/md0
/dev/md0:
        Version : 1.2
  Creation Time : Thu Mar  9 14:46:13 2023
     Raid Level : raid10
     Array Size : 41909248 (39.97 GiB 42.92 GB)
  Used Dev Size : 20954624 (19.98 GiB 21.46 GB)
   Raid Devices : 4
  Total Devices : 4
    Persistence : Superblock is persistent

    Update Time : Thu Mar  9 14:49:17 2023
          State : active, resyncing
 Active Devices : 4
Working Devices : 4
 Failed Devices : 0
  Spare Devices : 0

         Layout : near=2
     Chunk Size : 512K

  Resync Status : 90% complete

           Name : linuxprobe.com:0  (local to host linuxprobe.com)
           UUID : 677b3bb3:5ab19c7d:9b2b8460:3dbacc6c
         Events : 15

    Number   Major   Minor   RaidDevice State
       0       8       16        0      active sync   /dev/sdb
       1       8       32        1      active sync   /dev/sdc
       2       8       48        2      active sync   /dev/sdd
       3       8       64        3      active sync   /dev/sde

```


##### 9. Set the default data storage location: You can set the default data storage location by changing the user's home directory, for example:
`sudo usermod -d /mnt/raid10/username username`
This will change **username**'s home directory to **/mnt/raid10/username** and the default data storage location to the RAID10 disk.

##### 10. 重启
All operations and resulting files should now be stored on the RAID10 disk.

attached
`fdisk /dev/sdb` manages disk partitions
