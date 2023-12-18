``` bash
sudo fdisk /dev/sdX
```

``` bash
sudo mkfs.ext4 /dev/sdXY
```

``` bash
sudo shred -n 3 -vz /dev/sdX
```

``` bash
sudo dd if=/dev/zero of=/dev/sdX bs=4M status=progress
```