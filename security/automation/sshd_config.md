# non_master server ansible_controlller 접근 허용 ip
``` bash
 ansible-console non_master -b
server@non_master (4)[f:50]# tail -n 1 /etc/ssh/sshd_config
10.10.1.2 | CHANGED | rc=0 >>
AllowUsers server@10.20.1.2
10.10.1.3 | CHANGED | rc=0 >>
AllowUsers server@10.20.1.2
10.40.1.2 | CHANGED | rc=0 >>
AllowUsers server@10.20.1.2
10.40.1.3 | CHANGED | rc=0 >>
AllowUsers server@10.20.1.2
```

# Master server 접근허용 ip
``` bash
server@master20-2:~/Document$ tail -n 3 /etc/ssh/sshd_config
AllowUsers server@10.20.1.2
AllowUsers server@192.168.35.168
AllowUsers server@10.100.1.2
```
