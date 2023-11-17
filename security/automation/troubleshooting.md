# ansible을 사용하는데 필요한 필수적인 trouble shooting
1. /etc/ansible/hosts의 ansible_user이름이 올바른가?
2. /etc/ansible/hosts의 패스워드를 암호화한 상태로 입력했는가?(암호화 하지않는 상태면은 default를 복호화 해서 전송해서 암호가 이상해지기도 함)
3. root가 필요한 옵션인데 -b라는 옵션을 주지않았는가?
4. -b 다음에 -K 대신에 -k를 입력하였는가?
5. 접속한 user가 sudo 권한을 가지지 못했는가?

# rule 5번에 대한 보안적 트러블슈팅
1. 다른 slave들이 root를 가질때 ssh로 접근하는 master를 한정시켰는가? -> root로 ssh가 접근할수있다면은 심각한 보안 취약점이 되므로 반드시 제한해라
2. 그 master가 일반 유저들이 쉽게 접근할수있는 권한을 가졌는가? -> 가졌다면 심각한 구조적 보안취약점이 될수있다. 

# 정책적, 보안적, 직접 접근하여도 문제가 없는데 ansible-playground만 안될떄
> sh라는 shell을 사용했을때 특정 tools을 사용이 가능한가?   

-> 기본적으로 shell이라는 module은 bash shell이 아닌  sh라는 shell입니다.  