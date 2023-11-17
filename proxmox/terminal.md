# proxmox에서 terminal로 접속하는 방법

``` bash
qm set 302 -serial0 socket
```

``` bash
qm reboot 302
```


# debian 에서 하는법

`/etc/systemd/system/serial-getty@ttyS0.service` 파일을 생성하고 편집합니다.  

``` bash
sudo nano /etc/systemd/system/serial-getty@ttyS0.service
```

``` bash
[Unit]
Description=Serial Getty on %I
Documentation=man:agetty(8) man:systemd-getty-generator(8)
After=systemd-user-sessions.service plymouth-quit-wait.service

[Service]
ExecStart=-/sbin/agetty --autologin root --noclear %I 115200 vt102
Type=idle
Restart=always
RestartSec=0
UtmpIdentifier=%I
TTYPath=/dev/%I
TTYReset=yes
TTYVHangup=yes
TTYVTDisallocate=yes
KillMode=process
IgnoreSIGPIPE=no
SendSIGHUP=yes

[Install]
WantedBy=getty.target
```


``` bash
sudo systemctl enable serial-getty@ttyS0.service
sudo systemctl start serial-getty@ttyS0.service
```

# `/etc/default/grub` 파일 열고 아래와같이 추가 

``` bash
GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200"
```


# grub update
``` bash
sudo update-grub
```