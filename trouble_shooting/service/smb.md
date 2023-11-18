# useradd -M -s /sbin/nologin user
# smbpasswd -a user                  -- for each user to activate the Samba user account and set a password.
# mkdir [-p]  <path/to/share>    -- make the directory for the share
# chmod -R 777 <path/to/share>   -- to provide the users read/write/execute permissions to the share directory.
# chcon -R -t samba_share_t <path/to/share>  --to set the SElinux contexts
# cd <path/to/share>
# echo test > testfile.txt

# User create

``` bash
useradd -M -s /sbin/nologin user
passwd user
smbpasswd -a user
smbpasswd -e user
```

# Path

``` bash
mkdir /smbshare
mount /dev/[device] /smbshare
chmod -R 777 /smbshare
chcon -R -t samba_share_t /smbshare
```

# smb.conf setting

`cp /etc/samba/smb.conf /tmp`
`vim /etc/samba/smb.conf`

``` bash
[local_share]
        path = /smbshare
        valid users = shsmb
        writable = yes
        browsable = yes
        create mask = 0777
        comment = samba share for company files
```

# Debuging

``` bash
testparm
smbcontrol all reload-config
```

# test success 

``` bash
firewall-cmd --permanent --add-service=samba
firewall-cmd --reload

systemctl enable --now smb
```

``` bash
service smb restart
service firewalld start
setenforce 1
```
 
# smb raw config file

`smb.conf`

``` bash
[global]
        workgroup = WORKGROUP
        security = user

        passdb backend = tdbsam

        printing = cups
        printcap name = cups
        load printers = yes
        cups options = raw

[homes]
        comment = Home Directories
        valid users = %S, %D%w%S
        browseable = No
        read only = No
        inherit acls = Yes

[printers]
        comment = All Printers
        path = /var/tmp
        printable = Yes
        create mask = 0600
        browseable = No

[print$]
        comment = Printer Drivers
        path = /var/lib/samba/drivers
        write list = @printadmin root
        force group = @printadmin
        create mask = 0664
        directory mask = 0775
[local_share]
        path = /smbshare
        valid users = shsmb
        writable = yes
        browsable = yes
        create mask = 0777
        comment = samba share for company files
```