# ftp mount 

``` bash
sudo apt-get install curlftpfs
sudo curlftpfs ftp://192.168.35.150 /mnt/myftp/ -o user=ftp:1234
```

# ftp server 종류
![iis](../img/iis.png)  
상대적으로 윈도우 NFS와같은거 사용하는거보단 편리하고 관리도 편하고 직관적임  
NFS보다 속도가 빠른편이고 공유폴더는 속도가 말도안되게 느려서 비추  어차피 mount하면 되기떄문에 실시간 전송측면에선 어차피


``` bash
fsck -Af
```
`all yes`
``` bash
sudo reboot
```

``` bash
mount -o remount,rw /dev/sda1 /
```