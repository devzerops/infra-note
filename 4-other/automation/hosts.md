# Linux
`/etc/ansible/hosts`
``` bash
[linux]
10.10.1.2
10.10.1.3
10.11.1.2
10.20.1.3
10.40.1.2
10.40.1.3

[linux:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_user=server
ansible_become_pass=1234
```

# windows

``` bash
[windows]
10.30.1.2
10.30.1.3
10.30.1.4

[windows:vars]
ansible_user=administrator
ansible_password=changeme
ansible_connection=winrm
ansible_winrm_server_cert_validation=ignore
ansible_winrm_transport=ntlm
ansible_port=5985

[windows:children]
windows
```