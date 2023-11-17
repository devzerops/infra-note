# ansible을 통한 네트워크 장애 제어

## hosts 문제

``` bash
echo "1234" | sudo -A hostnamectl set-hostname server
```

``` bash
echo "1234" | sudo -S hostnamectl set-hostname server
```


## network/interfaces 설정문제

``` bash
sudo cp /etc/network/interfaces /etc/network/interfaces.bak  
sudo sed -i -E '/dns-nameservers/s/10\.[0-9]+\.[0-9]+\.[0-9]+//g' /etc/network/interfaces  # 10.*.*.* 쓸데없는 dns-nameservers 설정제거
```

## ip 추출 Subnetting 포함 

``` bash
 ip addr show ens192 | grep 'inet\b' | awk '{print $2}'
```

## 데몬을 통한 network 실패 제어 

``` bash
#!/bin/bash

# Monitor the network interface status
INTERFACE="ens192"
LOG_TAG="network-monitor"

while true; do
  if ifconfig "$INTERFACE" | grep -q "UP"; then
    logger -t "$LOG_TAG" "Network interface $INTERFACE is up"
  else
    logger -t "$LOG_TAG" "Network interface $INTERFACE is down, restarting..."
    ifdown "$INTERFACE" && ifup "$INTERFACE"
  fi
  sleep 60
done
```

