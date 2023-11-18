# Firefox

Firefox는 기본적으로 프록시서버를 세팅하더라도 따로 설정에 들어가서 활성화를 해주지 않으면은   
프록시서버를 사용하지않게 해준다. 그러니 squidguard를 실습하고싶으면은 아래와 같이 해주자.


# proxy 확인

``` bash
set | grep -i proxy
```