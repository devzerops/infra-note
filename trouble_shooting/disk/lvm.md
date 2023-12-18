``` bash
root@pve:~# swapoff -a
root@pve:~# lvchange -a n /dev/pve/swap
root@pve:~# lsblk
NAME         MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda            8:0    0   1.8T  0 disk 
sdb            8:16   0 931.5G  0 disk 
├─sdb1         8:17   0  1007K  0 part 
├─sdb2         8:18   0     1G  0 part /boot/efi
└─sdb3         8:19   0 930.5G  0 part 
  └─pve-root 252:1    0    96G  0 lvm  /
nvme0n1      259:0    0 931.5G  0 disk 
root@pve:~# lvremove /dev/pve/swap
  Logical volume "swap" successfully removed.
```








``` bash
root@pve:/dev/pve# lsblk
NAME         MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda            8:0    0   1.8T  0 disk 
sdb            8:16   0 931.5G  0 disk 
├─sdb1         8:17   0  1007K  0 part 
├─sdb2         8:18   0     1G  0 part /boot/efi
└─sdb3         8:19   0 930.5G  0 part 
  └─pve-root 252:1    0    96G  0 lvm  /
nvme0n1      259:0    0 931.5G  0 disk 
root@pve:/dev/pve# lvcreate -l +100%FREE -n low-load pve
```

``` bash
root@pve:/dev/pve# lsblk
NAME            MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda               8:0    0   1.8T  0 disk 
sdb               8:16   0 931.5G  0 disk 
├─sdb1            8:17   0  1007K  0 part 
├─sdb2            8:18   0     1G  0 part /boot/efi
└─sdb3            8:19   0 930.5G  0 part 
  ├─pve-lowLoad 252:0    0 834.5G  0 lvm  
  └─pve-root    252:1    0    96G  0 lvm  /
nvme0n1         259:0    0 931.5G  0 disk 
```

``` bash
mkfs.ext4 /dev/pve/low-load

root@pve:/dev/pve# echo '/dev/pve/lowLoad /var/lib/vz ext4 defaults 0 2' >> /etc/fstab
root@pve:/dev/pve# systemctl daemon-reload
```