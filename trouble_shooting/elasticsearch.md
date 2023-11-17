# 메모리 부족현상
메모리 부족현상으로 Elasticsearch가 강제종료됨
``` bash
┌──(root㉿siem)-[~/zeek]
└─# systemctl status suricata

× elasticsearch.service - Elasticsearch
     Loaded: loaded (/lib/systemd/system/elasticsearch.service; enabled; preset: disabled)
     Active: failed (Result: oom-kill) since Fri 2023-05-12 09:34:06 EDT; 1h 28min ago
   Duration: 19h 9min 42.711s
       Docs: https://www.elastic.co
   Main PID: 5255 (code=exited, status=143)
        CPU: 5h 29min 7.655s

May 11 14:23:47 siem systemd[1]: Starting elasticsearch.service - Elasticsearch...
May 11 14:24:14 siem systemd[1]: Started elasticsearch.service - Elasticsearch.
May 12 09:33:57 siem systemd[1]: elasticsearch.service: A process of this unit has been killed by the>
May 12 09:34:02 siem systemd-entrypoint[5255]: ERROR: Elasticsearch exited unexpectedly
May 12 09:34:06 siem systemd[1]: elasticsearch.service: Failed with result 'oom-kill'.
May 12 09:34:06 siem systemd[1]: elasticsearch.service: Consumed 5h 29min 7.655s CPU time.
```


# root 문제

기존 사용자도 `sudo systemctl restart elastic` 해도됨
관리하기 편하라고 분리하는거

``` bash
useradd -m elastic -s /bin/zsh -G sudo -U
sudo useradd -m elastic -s /bin/zsh -G sudo -U
passwd elastic
sudo passwd elastic
```

``` bash
sudo systemctl restart elastic
```