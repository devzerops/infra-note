# Security Onion이란?

도커를 메인으로 활용한 통합 IDS라고도 할 수 있다.  
공식사이트에서 설명하길 위협감지, 네트워크 모니터링, 로그 관리를 할수있는 보안  통합솔루션이라 말한다.  
pfsense보다 한층 더 복잡하고 정교한 로그 모니터링을 할 수 있으며 다양한 그래프를 보여준다. 당연하겠지만 pfsense는 IDS라고 하기앤 너무 애매한 모니터링 성능을 보여준다.  
Suricata, Zeek, Wazuh, Elastic Stack과 다른 확장 기능들도 지원하고 추가도 가능하다.  
주요 설치 내용은 말하겠지만 유튜브를 보고 영상을 보면서 따라하길 추천한다.  



# Local의 ip들의 SO의 접근 허용방법

sudo so-allow  -> a -> 아이피 대역 입력(특정 ip도 됨)  
아이피는 10.10.10.0/24처럼 서브네팅까지 포함하여 대역을 입력해서 특정 대역의 접속을 허용할수도 있고 /32로 해서 호스트 한개만 허용해도되고   
아에 서브넷을 입력하지않고 호스트 한개만 접속을 허용하게 할 수도 있다.  


# 
``` bash
so-rule-update
```

# 


![img0](./img/0.png)  
![img1](./img/1.png)  
![img2](./img/2.png)  
![img3](./img/3.png)  
![img4](./img/4.png)  
![img5](./img/5.png)  
![img6](./img/6.png)  
![img7](./img/7.png)  
![img8](./img/8.png)  
![img9](./img/9.png)  
![img10](./img/10.png)  
![img11](./img/11.png)  
![img12](./img/12.png)  
![img13](./img/13.png)  
![img14](./img/14.png)  
![img15](./img/15.png)  
![img16](./img/16.png)  
![img17](./img/17.png)  
![img18](./img/18.png)  
![img19](./img/19.png)  
![img20](./img/20.png)  
![img21](./img/21.png)  
![img22](./img/22.png)  
![img23](./img/23.png)  
![img24](./img/24.png)  
![img25](./img/25.png)  
![img26](./img/26.png)  
![img27](./img/27.png)  
![img28](./img/28.png)  
![img29](./img/29.png)  
![img30](./img/30.png)  
![img31](./img/31.png)  
![img32](./img/32.png)  
![img33](./img/33.png)  
![img34](./img/34.png)  
![img35](./img/35.png)  
![img36](./img/36.png)  

security onion을 제대로 설치했다면 기본적으로 터미널환경이 제공됩니다. 최근에는 터미널을 기본으로 하고 analogy라는 스크립트를 실행하여 설치할시에   
이전 보안양파와 같은 환경 즉 desktop 환경을 사용할수있습니다.  결정적인 차이점은 대부분의 프로그램은 도커위에서 띄운다는것 입니다.  
여기서 자세히 봐야할점은 suricata만을 사용한다는 점이고 snort는 지원하지 않는다는 점 입니다.  
하드웨어의 자원이 부족하면은 통상적으로 PPS(Packet Per Seconds) 처리속도가 snort가 훨씬 좋습니다만 
하드웨어의 자원이 증가할수록 목적별 하드웨어 자원을 선택할수있는 분산멀티쓰레딩 방식을 활용하는 suricata가 PPS가 눈에 띄게 높아집니다.  
그렇기에 suricata를 security onion에서 활용합니다. 아래 그래프는 suricata와 snort의 하드웨어 자원별 PPS를 처리하는 증가량을 나타내는 그래프입니다.  

![IDS](./img/IDS.png)

# 그외 알아두면 좋은사항

suricata, snort 모두 돈을내면은 주기적으로 최신 이슈에 맞춰 룰을 업데이트해주는 enterpirce 정책이 있습니다.  
이러한 정책들을 사용하여 효과적으로 최신의 알려진 취약점들을 빠르게 막을수있습니다.  

# SO WEB 접근방법

``` bash
so-allow 
```