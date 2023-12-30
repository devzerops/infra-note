# Proxmox Vlan 설정방법 

기본적으로 Proxmox에는 리눅스기반이다 보니까 권장되는 두가지 네트워크 설정 방법이 있다.  

# 1. 기존 리눅스 설정을 활용한 방법 
![Linux Default Network Configuration](https://pve.proxmox.com/wiki/Network_Configuration)

``` bash
vi /etc/network/interfaces
```

# 2. 리눅스의 OVS란 다른 패키지를 이용하는 방법 
![OVS Network Configuration](https://pve.proxmox.com/wiki/Open_vSwitch)

``` bash
apt update
apt install openvswitch-switch
```

확실히 숙련도의 차이인지는 몰라도 Linux  Default NEtwork로는 ESXI에서 했던 세팅을 할수가 없었다.  
그래서 OVS라는 리눅스내 가상 스위치를 활용한 VLAN 망 분리를 진행하였다.  

``` bash
auto lo
iface lo inet loopback

auto enx00e04c680010
iface enx00e04c680010 inet manual
        ovs_type OVSPort
        ovs_bridge vmbr0

auto enp39s0
iface enp39s0 inet manual
        ovs_type OVSPort
        ovs_bridge vmbr1

auto vlan10
iface vlan10 inet manual
        ovs_type OVSIntPort
        ovs_bridge vmbr2
        ovs_options tag=10

auto vlan20
iface vlan20 inet manual
        ovs_type OVSIntPort
        ovs_bridge vmbr2
        ovs_options tag=20

auto vlan30
iface vlan30 inet manual
        ovs_type OVSIntPort
        ovs_bridge vmbr2
        ovs_options tag=30

auto vlan40
iface vlan40 inet manual
        ovs_type OVSIntPort
        ovs_bridge vmbr2
        ovs_options tag=40

auto vlan50
iface vlan50 inet manual
        ovs_type OVSIntPort
        ovs_bridge vmbr0
        ovs_options tag=50
#honeypot

auto vlan60
iface vlan60 inet manual
        ovs_type OVSIntPort
        ovs_bridge vmbr2
        ovs_options tag=60
#SIEM

auto vmbr0
iface vmbr0 inet static
        address 192.168.35.250/24
        gateway 192.168.35.1
        ovs_type OVSBridge
        ovs_ports enx00e04c680010 vlan50
#proxmox

auto vmbr1
iface vmbr1 inet manual
        ovs_type OVSBridge
        ovs_ports enp39s0
#WAN 

auto vmbr2
iface vmbr2 inet manual
        ovs_type OVSBridge
        ovs_ports vlan10 vlan20 vlan30 vlan40 vlan60
#LAN
```


# SPAN 설정 방법 

SPAN은 간단하게 논리적으로 다른 트래픽을 모니터링하는 방법이다.  
OVS에서도 가상이라도 스위치다 보니까 이 기능을 지원한다.  
하지만 문제점이 VM을 켤때마다 지속적으로 SPAN포트 설정을 적용해야하는 번거로움이 있다.  
그래서 proxmox에선 hookscript라고 vm을 실행할때마다 실행할수있도록 도와주는 기능이 있다.  
아래는 SPAN으로 포트를 미러링하는 방법이 있다.  

# setting

`create port-mirror.sh script`
``` bash
vi /var/lib/vz/snippets/port-mirror.sh
```

``` bash
chmod +x /var/lib/vz/snippets/port-mirror.sh
```

``` bash
qm set 6003 --hookscript local:snippets/port-mirror.sh
```

# port-mirror scripts

`주의사항` /root/scripts/ 파일이 없으면 스크립트를 실행할수없다. 예외처리를 해줘야하는데 일단 이렇게 놔뒀다.

``` bash
#! /usr/bin/env bash

VM_ID=$1
EXECUTION_PHASE=$2
VM_BRIDGE="vmbr2"
LOGGING=/root/scripts/port-mirror.log


function create_mirror {
  /usr/bin/date >> $LOGGING
  /usr/bin/echo "Creating mirror on $VM_BRIDGE for $VM_ID" >> $LOGGING
  /usr/bin/ovs-vsctl \
    -- --id=@tap"$VM_ID"i0 get Port tap"$VM_ID"i0 \
    -- --id=@"$VM_ID"m create Mirror name="$VM_ID"-mirror \
      select-all=true \
      output-port=@tap"$VM_ID"i0 \
    -- set Bridge "$VM_BRIDGE" mirrors=@"$VM_ID"m >> $LOGGING
  /usr/bin/echo "####################" >> $LOGGING
}

function clear_mirror {
  /usr/bin/date >> $LOGGING
  /usr/bin/echo "Clearing mirror on $VM_BRIDGE for $VM_ID" >> $LOGGING
  /usr/bin/ovs-vsctl \
    -- --id=@"$VM_ID"m get Mirror "$VM_ID"-mirror \
    -- remove Bridge "$VM_BRIDGE" mirrors @"$VM_ID"m >> $LOGGING
  /usr/bin/echo "####################" >> $LOGGING
}

function show_mirrors {
  /usr/bin/date >> $LOGGING
  /usr/bin/echo "Show existing mirrors" >> $LOGGING
  /usr/bin/ovs-vsctl list Mirror >> $LOGGING
  /usr/bin/echo "####################" >> $LOGGING
}

if [[ "$EXECUTION_PHASE" == "post-start" ]]; then
  sleep 30 
  create_mirror
  show_mirrors
elif [[ "$EXECUTION_PHASE" == "pre-stop" ]]; then
  clear_mirror
  show_mirrors
fi

```