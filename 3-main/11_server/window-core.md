# DNS 설정방법
``` bash
netsh interface ipv4 set dns name=Ethernet0 source=static address=10.30.1.2 register=primary
```