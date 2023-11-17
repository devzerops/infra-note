``` bash





sudo apt install cloud-guest-utils parted


parted
root@node11-2 / # parted
GNU Parted 3.4
Using /dev/sda
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) print
Model: VMware Virtual disk (scsi)
Disk /dev/sda: 26.8GB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags:

Number  Start   End     Size    Type      File system     Flags
 1      1049kB  20.5GB  20.4GB  primary   ext4            boot
 2      20.5GB  21.5GB  1022MB  extended
 5      20.5GB  21.5GB  1022MB  logical   linux-swap(v1)
 3      21.5GB  26.8GB  5370MB  primary   ext4

(parted) rm 2

(parted) rm 3
(parted) q

root@node11-2 / # partprobe /dev/sda
root@node11-2 / # growpart /dev/sda 1
CHANGED: partition=1 start=2048 old: size=39940096 end=39942144 new: size=52426719 end=52428767
root@node11-2 / # lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0   25G  0 disk
└─sda1   8:1    0   25G  0 part /
sr0     11:0    1  382M  0 rom







root@node11-2 / # resize2fs /dev/sda1
resize2fs 1.46.2 (28-Feb-2021)
Filesystem at /dev/sda1 is mounted on /; on-line resizing required
old_desc_blocks = 3, new_desc_blocks = 4
The filesystem on /dev/sda1 is now 6553339 (4k) blocks long.
```


``` bash
sudo apt install cloud-guest-utils parted
```

``` bash
parted
```

``` bash
print 
rm (number)
```

``` bash
partprobe /dev/sda
growpart /dev/sda 1
```

``` bash
resize2fs /dev/sda1
```

# swap
swap인데 elastic보니까 별로 메모리 비중은 높진않아서
굳이 하면서 swap 빼버림
