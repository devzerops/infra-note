# 리눅스 우선순위
이러한 문제는 리눅스에서 파일을 읽고 쓰는 방식 때문에 발생합니다.

아래에서 사용한 명령어에서는 ">" (리다이렉션) 기호를 사용하여 /etc/resolv.conf 파일에 데이터를 덮어쓰기 하려고 합니다. 그러나 리눅스에서 파일 쓰기 작업은 다음과 같이 동작합니다.

먼저 운영체제는 파일을 연다.
그리고 파일에 대한 파일 디스크립터(File Descriptor)를 생성합니다.
이후 파일 디스크립터를 사용하여 파일을 읽거나 쓸 수 있습니다.
따라서 ">" 기호로 파일을 덮어쓰는 경우, 리눅스는 다음과 같은 순서로 동작합니다.

먼저 운영체제는 /etc/resolv.conf 파일을 연다.
그리고 파일에 대한 파일 디스크립터를 생성합니다.
">" 기호 오른쪽에 있는 명령어가 실행됩니다.
이때, 파일 디스크립터를 이용하여 파일의 내용을 지우고, 명령어로부터의 데이터를 쓰려고 합니다.
그러나 명령어에서 사용된 데이터가 아직 읽히기 전에, 파일 디스크립터로부터 파일 내용을 삭제하려고 하므로, 파일 내용이 비게 됩니다.
그리고 ">" 기호 오른쪽에 있는 명령어는 오류가 발생하게 됩니다.
따라서 파일의 내용을 덮어쓰는 작업을 할 때에는 위에서 설명한 대로 일어나는 순서를 고려하여 명령어를 작성해야 합니다.


# 에러 발생
``` bash
server@all (8)[f:200]# cat /etc/resolv.conf | grep -v 169 > /etc/resolv.conf
10.11.1.2 | FAILED | rc=1 >>
non-zero return code
10.40.1.2 | FAILED | rc=1 >>
non-zero return code
10.10.1.3 | FAILED | rc=1 >>
non-zero return code
10.11.1.3 | FAILED | rc=1 >>
non-zero return code
10.20.1.2 | FAILED | rc=1 >>
non-zero return code
10.20.1.3 | FAILED | rc=1 >>
non-zero return code
10.20.1.4 | FAILED | rc=1 >>
non-zero return code
10.10.1.2 | FAILED | rc=1 >>
non-zero return code
```
`파일이 비어있음`
``` bash
server@all (8)[f:200]# cat /etc/resolv.conf
10.10.1.2 | CHANGED | rc=0 >>

10.11.1.2 | CHANGED | rc=0 >>

10.11.1.3 | CHANGED | rc=0 >>

10.10.1.3 | CHANGED | rc=0 >>

10.40.1.2 | CHANGED | rc=0 >>

10.20.1.2 | CHANGED | rc=0 >>

10.20.1.4 | CHANGED | rc=0 >>

10.20.1.3 | CHANGED | rc=0 >>
```
# 에러 발생 안함
``` bash
server@all (8)[f:200]# cat /etc/resolv.conf | grep -v 169 > /etc/resolv.conf.tmp && mv /etc/resolv.conf.tmp /etc/resolv.conf
10.20.1.2 | CHANGED | rc=0 >>

10.20.1.3 | CHANGED | rc=0 >>

10.10.1.2 | CHANGED | rc=0 >>

10.10.1.3 | CHANGED | rc=0 >>

10.20.1.4 | CHANGED | rc=0 >>

10.11.1.3 | CHANGED | rc=0 >>

10.11.1.2 | CHANGED | rc=0 >>

10.40.1.2 | CHANGED | rc=0 >>
```

``` bash

server@all (8)[f:200]# cat /etc/resolv.conf
10.10.1.3 | CHANGED | rc=0 >>
# Ansible entries BEGIN
domain cluster.local
search default.svc.cluster.local svc.cluster.local
nameserver 8.8.8.8
options ndots:2 timeout:2 attempts:2
# Ansible entries END
10.11.1.2 | CHANGED | rc=0 >>
# Ansible entries BEGIN
domain cluster.local
search default.svc.cluster.local svc.cluster.local
nameserver 8.8.8.8
options ndots:2 timeout:2 attempts:2
# Ansible entries END
10.11.1.3 | CHANGED | rc=0 >>
# Ansible entries BEGIN
domain cluster.local
search default.svc.cluster.local svc.cluster.local
nameserver 8.8.8.8
options ndots:2 timeout:2 attempts:2
# Ansible entries END
10.20.1.2 | CHANGED | rc=0 >>
# Ansible entries BEGIN
domain cluster.local
search default.svc.cluster.local svc.cluster.local
nameserver 8.8.8.8
options ndots:2 timeout:2 attempts:2
# Ansible entries END
10.40.1.2 | CHANGED | rc=0 >>
# Ansible entries BEGIN
domain cluster.local
search default.svc.cluster.local svc.cluster.local
nameserver 8.8.8.8
options ndots:2 timeout:2 attempts:2
# Ansible entries END
10.20.1.4 | CHANGED | rc=0 >>
# Ansible entries BEGIN
domain cluster.local
search default.svc.cluster.local svc.cluster.local
nameserver 8.8.8.8
options ndots:2 timeout:2 attempts:2
# Ansible entries END
10.20.1.3 | CHANGED | rc=0 >>
# Ansible entries BEGIN
domain cluster.local
search default.svc.cluster.local svc.cluster.local
nameserver 8.8.8.8
options ndots:2 timeout:2 attempts:2
# Ansible entries END
10.10.1.2 | CHANGED | rc=0 >>
# Ansible entries BEGIN
domain cluster.local
search default.svc.cluster.local svc.cluster.local
nameserver 8.8.8.8
options ndots:2 timeout:2 attempts:2
# Ansible entries END
```
