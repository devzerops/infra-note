# ansible_console

ansible_console은 대충 명령어를 실시간으로 실행하게 해주는 도구다.  다양한 클라이언트들에게 간단한 작업들을 수행시킬떄 유용하다.  
이 기능을 바탕으로 간단하게 할수있는 보안점검을 할것이다.  

# 점검
가장 기본적인 점검인 passwd로 bash가 인가되지 않는 사용자에게 할당됐는지 점검하는것을 수행할것이다.  
보통 저런 기본적인건 수동으로 점검하진 않고 자동화 시키지만 배쉬 명령어들을 한번 본다는 관점에서 적어봤다.  

## /etc/passwd
``` bash
ansible all -m shell -a "cat /etc/passwd | grep bash " > passwd.txt.origin &&\
sed '/CHANGED/{x;p;x;}' passwd.txt.origin > passwd.txt &&\
rm -f  passwd.txt.origin
```

## /etc/shadow

``` bash
ansible all -m shell -a "cat /etc/shadow" -b -K > shadow.txt.origin &&\
sed '/CHANGED/{x;p;x;}' shadow.txt.origin > shadow.txt &&\
rm -f  shadow.txt.origin
```

## ufw status

``` bash
ansible all -m shell -a "ufw status" -b -K > ufw.txt &&\
sed '/CHANGED/{x;p;x;}' ufw.txt.origin > ufw.txt &&\
rm -f ufw.txt.origin
```

## group

``` bash
ansible all -m shell -a "cat /etc/group" -b -K > group.txt &&\
sed '/CHANGED/{x;p;x;}' group.txt.origin > group.txt &&\
rm -f group.txt.origin
```

# lynsis

기본적인 보안 결함들을 빠르게 찾아주는 도구로 주정통, 국정원 메뉴얼처럼 타이트하지는 않지만  
자동화라는 관점으로 빠르게 기본적인 보안적 결함을 찾아주는 도구로서 실무자들의 말에 따르면은 금융권에서도 자주사용하는 도구라고 한다.  
오픈소스이며 표준 패키지에 항상 포함되있는 등 다양한 장점으로 여러가지로 사용되는 도구이다.  
우리는 이 도구를 사용할것이다.  

간단하게 심각 수준의 보안 취약점을 스캔을 해주자.  

``` bash
ansible all -m shell -a "lynis audit system --warnings-only" -b -K > WARNING.txt
```

> WARNING.txt

``` bash
10.10.1.4 | CHANGED | rc=0 >>
    - Permissions for directory: /etc/sudoers.d               [ WARNING ]
    - Minimal of 2 responsive nameservers                     [ WARNING ]
  - Permissions of home directories                           [ WARNING ]
10.10.1.2 | CHANGED | rc=0 >>
    - Permissions for directory: /etc/sudoers.d               [ WARNING ]
  - Checking vulnerable packages                              [ WARNING ]
    - Minimal of 2 responsive nameservers                     [ WARNING ]
  - Permissions of home directories                           [ WARNING ]
10.40.1.2 | CHANGED | rc=0 >>
    - Permissions for directory: /etc/sudoers.d               [ WARNING ]
  - Checking vulnerable packages                              [ WARNING ]
    - Minimal of 2 responsive nameservers                     [ WARNING ]
  - Permissions of home directories                           [ WARNING ]
10.11.1.2 | CHANGED | rc=0 >>
    - Permissions for directory: /etc/sudoers.d               [ WARNING ]
    - Minimal of 2 responsive nameservers                     [ WARNING ]
  - Permissions of home directories                           [ WARNING ]
  - Ownership of home directories                             [ WARNING ]
10.10.1.3 | CHANGED | rc=0 >>
    - Permissions for directory: /etc/sudoers.d               [ WARNING ]
    - Minimal of 2 responsive nameservers                     [ WARNING ]
  - Permissions of home directories                           [ WARNING ]
```
1. 기본적으로 ansible을 위한 sudo 세팅으로 인하여 심각한 보함결함이 나타났다.
2. 네임서버가 최소 2개이상이어여 한다.
3. 홈 디렉터리의 사용권한이 비정상적이다.
4. 홈 디렉터리 소유권이 비 정상적이다.

즉 4개의 케이스가 있는데 실제로는 여러 의견을 조율하면서 실 서비스가 문제되지 않게 하면서 고쳐야 하지만  
우리는 가상의 환경을 사용하고있으며 간단한 서비스를 띄우고 하기때문에 자동화시켜서 이 문제를 해결할것이다.
그전에 이 문제점들이 왜 심각한 취약점인지 먼저 예상먼저 해보자

1. 이 케이스는 당연하게도 일반 유저에 우리는 아무런 제한과 규정없이 허용했기에 해커가 이 sudo 권한을 가진 사용자의 비밀번호를 탈취할시에  
그 서비스의 모든 제어권이 넘어가게된다. 참고로 이 서비스들을 통합적으로 관리하는 ansible controler인 master에 연결권한도 제한하지 않았기에  
최악에는 해커가 모든 제어권을 장악하고 한번에 모든 서비스를 제어할수 있게된다.  
2. 네임서버가 2개이상이지 않으면은 DNS Spoofing, Poisoning,같은 공격에 취약해지고 한개의 DNS가 이상이 생기면은 서비스에 장애가 생기는 심각한 문제가 발생할수 있다.
3. 홈 디렉터리의 사용자 권한, 소유권은 정상적이지 않으면 인가되지 않은 사용자가 접근하여 파일의 I-node를 관찰할수 있는 취약점, 심각하면은 허가되지 않는 파일에 접근하거나 비정상적인 프로그램도 허용할수있게 되므로 반드시 정상적인 소유권을 가진 사용자로 만들어야한다.