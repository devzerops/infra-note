# ansible 속도개선 방법
[redhat의 속도개선방법](https://www.redhat.com/sysadmin/faster-ansible-playbook-execution)  

서버와 네트워크에 부하는 좀 많이가기에 트래픽이 많은 상황에서는 또는  
보안정책에 따라서 차단당할수있는지에 따라서 사용하시길 바랍니다.  

> /etc/ansible/ansible.cfg
``` bash
[defaults]
host_key_checking = False
pipelining = True
forks=50
ssh_args = -o ControlMaster=auto -o ControlPersist=60s PreferredAuthentications=publickey
callbacks_enabled = timer, profile_tasks, profile_roles
```


``` bash
time ansible-playbook all_delete.yml -b -K --forks 50
```

# 옵션별 특징

* `callbacks_enabled`: 작업의 시간을 분석하여 느려지는 작업을 찾아서 옮겨주는 역할을 하는 플러그인
* `forks`: 말그대로 병렬처리입니다. forks의 개수에 따라 서버 자원에 부담이 직접적으로 증가하지만 그만큼 눈에띄게 속도 개선이 됩니다.  <br> 참고로 위와 같이 딱히 cfg에서 설정하지않아도 options로 조절 가능한 동적변수기도 해요
* `ssh_args`: ssh 자체가 원래 느린데 그것을 개선시켜주는 방법입니다. 근데 서버의 보안설정에 따라서 안될수도 있습니다.  
    * `ControlMaster`: 단일 호스트에 여러개의 세션을 연결가능하게 해주는 역할입니다.
    * `ControlPersist`: ssh의 연결을 idle 상태로 유지시켜줍니다.
* `host_key_checking`: 이것은 호스트를 확인하것을 물어보는 옵션인데 redhat에서 권장하지 않는 방법으로 보통 중간자 공격같은것에 취약합니다.
* `pipelining`: 명령을 복사하거나 준비를 백그라운드에서 실행합니다. ssh의 연결을 효율적으로 하여 오버헤드를 줄이는 기능을 합니다.  


``` yml
- hosts: production servers
  strategy: free
  tasks:
```
원래 ansible의 default는 선형 작업인데 이것은 비선형으로 작업을 처리하여 속도를 많이 개선시킬수 있습니다.  


``` yml
- hosts: all
  gather_facts: no
  tasks:
```
각 클라이언트들의 프로세스, os, cpu 정보를 수집하는데  
이것또한 끈다면은 초기에 뜨는 gather_facts의 시간을 줄여줍니다.  


``` yml
​​​​---
- name: Async Demo
  hosts: nodes
  tasks:
    
    - name: Initiate custom snapshot
      shell:
        "/opt/diskutils/snapshot.sh init"
      async: 120 # Maximum allowed time in Seconds
      poll: 05 # Polling Interval in Seconds
```
동기 비동기를 이용한 속도개선입니다.  

# 그외 속도개선 방법

1. `쉘 명령어를 최소화하기`, 모듈을 사용하여 작업을 처리하면은 쉘 명령어보다 작업속도가 크게 개선됩니다.  
2. `Mitogen Plugin`: ansible의 성능을 개선시켜주는 플러그인입니다. 속도향상이 크게 증가하며 ssh 터널링과 cpu rem 등을 효율적으로 사용합니다. ansible 계열에서 유명하더라고요.  
3. 서버의 하드웨어 향상: 이건 누구나 아는데 누구나 쉽게 못하는 방법이죠. 


# mitogen 설치하는법

[mitogen](https://mitogen.networkgenomics.com/ansible_detailed.html)  
여기 들어가서 tar.gz를 받은다음 압축을 풀어줍니다.  

``` bash
root@node1:/home/server# tar -zxvf mitogen-0.2.9.tar.gz
```

> vim /etc/ansible/ansible.cfg
``` bash
strategy_plugins = /home/server/mitogen-0.2.9/ansible_mitogen/plugins/strategy
```
아래 경로에 따라 다음과같이 작성만 하면 됩니다.
