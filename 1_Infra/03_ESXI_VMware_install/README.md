# 준비사항
1. pfsense.iso
2. Ubuntu22.04LTS.iso


# 시작

![vSphere ip](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FN9lUX%2FbtrO44MD8Vp%2FgdYJlsq0IVTWXE2Cdvrl00%2Fimg.png)

![VsPHERE](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbIogd4%2FbtrO6icmmP2%2FkGq24JEnT1znKifFiFRj61%2Fimg.png)

![EXSI](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb9HTH3%2FbtrO3FtPtKV%2FaOM447PdmdRInPMZgRhpU1%2Fimg.png)


위와같이 접속이 됐으면 성공적으로 된것이다.<br>
그러면 본격적으로 시작하기에 앞서서 스냅샷을 찍어주는게 좋다.

![snapshot](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F1ppQW%2FbtrODPjp9IE%2Fj1NZTU8FzwTDTBaAeTeyK0%2Fimg.png)

나는 미리 했지만 기본적으로는 스냅샷이 안찍혀있을것이다.<br>
스냅샷은 설정을 잘못하거나 했을때 과거로 되돌릴수가 있어서 매우 안정적으로 작업을 수행할 수가 있게된다.<br> 도커의 컨테이너를 이미지로 바꾸는 느낌이라 생각하면 된다.

# 네트워크 설정

우리는 아래와 같은 네트워크 구조를 구성할 것이다. 
![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbPzGlA%2FbtrOQDWjkJW%2FKUXV8O8wVQv0C6Blgk3Xwk%2Fimg.png) 


아래와 같이 네트워킹에 들어가준다.

![IMG](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbm4AIF%2FbtrOQqvTiTd%2FQLyhzVl3YsN1tCzYftAqxk%2Fimg.png)

포트 그룹에 가상스위치를 바인딩 하기 위해 일단 가상 스위치 먼저 생성을 해 준다.
<br>LAN처럼 WAN도 똑같이 생성해주면 된다.
<br>
![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcbfdBR%2FbtrOQFfv5RW%2FqseXJLJoO0SjWzmpGOxre1%2Fimg.png)


포트 그룹에 가상 스위치를 바인딩 해준다.<br>
마찬가지로 WAN도 똑같이 만들어 주면은 된다.
<br> 당연히 `WAN이면 가상스위치도 WAN으로 바인딩`
<br>
![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbMEnUa%2FbtrO4tMFo0w%2F9zz6o3bzWQQtKATmpNXQA1%2Fimg.png)

그러면 아래와 같이 업링크와 포트 그룹이 추가되는걸 볼 수가 있다.<br>
만약 아래처럼 되어있지 않으면은 처음으로 돌아가 네트워크를 다시 설정해주길 바란다.

# 운영체제 설치
pfsense와 ubuntu 이미지는 설치했다는 가정하에 진행

스토리지의 `datastore1`에 들어간다음 위와같이 들어가서 .iso로 pfsense와 우분투의 운영체제를 EXSI 서버에 업로드 해주면은 된다.


![os](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbfJlvI%2FbtrOEqjcBLH%2FMTK9SDBMbsdIvHPkfBv56k%2Fimg.png)


그리고 아래와 같이 가상시스템에 들어간다음 VM 생성/등록을 클릭한다.
옵션도 아래와 같이 만들어주면은 된다.

![pfsense](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb3gXZL%2FbtrO3FtQLI7%2FOuIQx3kKKRGyalQwp7PiA1%2Fimg.png)

그리고 쭉 넘기다 보면은 아래와 같은 옵션선택창이 나오는데
<br> CPU와 메모리 하드는 대충 저리 맞춰주면된다. 실제 방화벽은 훨씬 고용량이지만
들어오는 디도스나 접속량 자체가 다르고 하니 저정도도 충분한 편이다.
<br> NIC은 `네트워크 어뎁터 추가`를 선택하면은 추가된다.
<br> 총 2개를 연결할것이다. `LAN`과 `WAN`이다.
<br> 2번 클릭하고 아래와같이 만들어준다. 그리고 쭉 다음으로 넘겨주면 완료된다

![pfsense](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbXWWgo%2FbtrO6icnjhJ%2FjKqlcFGppXiIWz1QDqNioK%2Fimg.png)

그리고 아래와 같이 실행해주고 pfsense 옵션에 뭐가 많이 뜨는데 그냥 놔두면 알아서 설치된다.

![pfsense1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fk7su4%2FbtrOQtzpSyQ%2FkcHtZqWS6wNqYMNBCMqfh0%2Fimg.png)


아래부터 쭉 따라해주면 된다
![pfsese](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdP4UBH%2FbtrO50C0Whi%2FAfkxeHusdJWon3QS5b5iU1%2Fimg.png)
![pfsense](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fk2KOV%2FbtrO3EV02Bk%2Fwy5dH2n9kM7XmU1wKDDZH0%2Fimg.png)
![pfsense](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbuDUlY%2FbtrO4gGRaMG%2F6fJkBnQkjcugTnSEsYrI81%2Fimg.png)
![pfsense](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FemXODP%2FbtrO3E2NnSY%2FaXBPnXOKLYGsK7OujFlew1%2Fimg.png)
![pfsense](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FemXODP%2FbtrO3E2NnSY%2FaXBPnXOKLYGsK7OujFlew1%2Fimg.png)
![pfsense](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbncYl6%2FbtrOEqDuK6e%2F7ZDktRWFsrbgZe6YfkndaK%2Fimg.png)
![pfsense](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsEfe3%2FbtrOEpYUDgG%2FO5TGs6a2OSfhAyjwDrJQXk%2Fimg.png)

# pfsense network 설정

LAN하고 WAN의 맥주소가 엇갈려있다. 8번은 누르고 SHELL에 들어가준다.

그리고 아래 명령어를 사용하고 pfsense 설정편집하고 대조해서 맥주소를 비교해준다.

만약 WAN하고 LAN하고 맥주소가 `비정상적으로 할당됐으면` 쭉 따라해주면 된다.
``` bash 
ifconfig | less
```
n
![pfsense](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmeKdt%2FbtrO3Fm6lXh%2F1rON9rczEWWl0iTcJi2HSk%2Fimg.png)


순서대로 쭉 쳐주면된다. 
Assign Interface를 들어가주고 WAN에 em1을 할당해주고
<br>LAN에 em0을 할당해서 맥주소를 제대로 잡아줄려는것이다.

1 -> n -> em1 -> em0 

![pfsense](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FWgrIR%2FbtrO45ELUw1%2FtQQ8Zoxjf0CwZuhoCNsvrK%2Fimg.png)

# pfsense LAN 재설정, 브라우저 활성화
192.168.1.1은 라우터 관리자 패널에 로그인하는데 사용하는 개인 ip주소다.

당연히 192.168.1.1에 lan에 설정되어있으면은 pfsense웹에 접속할수 없으므로 변경해준다.

물론 설정하면은 변경할 수는 있다. 기본적으론 안되지만.

아래처럼 해주자

2 ->192.168.2.1 -> 24 -> 엔터 -> 엔터 -> y -> 192.168.2.2 -> 192.168.2.254 -> 엔터
![pfsense](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FP79zX%2FbtrO3UYAUjz%2FYrkKykKR0eW0TDJMKcO271%2Fimg.png)
![pfsense](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcVpLFo%2FbtrO3VwqzO3%2FLlT4gusTa8OdQoK8z1f8QK%2Fimg.png)


# Lan을 사용하는 VM설치
pfsense처럼 설치해주면 된다.
다만 ubuntu는 LAN만을 사용할것이기에 LAN만 추가해준다.

![ubuntu](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Flx08f%2FbtrO4f8Yv0h%2FzvZfkbHaHtoj3tkFyJgl01%2Fimg.png)
![ubuntu](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbp3SET%2FbtrO4gmBzEt%2Fwk9Ykk8P3JInpVZwiHdpO1%2Fimg.png)

# pfsense 접속
우분투 설치법은 많으니 스킵하고 정상적으로 우분투가 설치되었으면은

192.168.2.1에 웹브라우저를 통하여 접속해주자

![ubuntu](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbtz3RS%2FbtrOQGeu6ql%2FRLiE8InJGjxy71RlhZPKt1%2Fimg.png)

로그인은 아래 정보를 보고 입력하면 된다.

## 아이디 비밀번호
* id: admin
* passwd: pfsense

로그인 하면은 설정하는 화면이 보이는데 아래 나오는 화면을 제외하고는 그냥 넘겨주면 된다.

![pfsense](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbwuz70%2FbtrO6hEysHu%2FJJGTkxHDULbtCziQqigZ30%2Fimg.png)

![pfsense](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FFjs6K%2FbtrO4hltYOI%2FNPSTduJUmfGu3kzKVyiDSk%2Fimg.png)

finish누르고 좀 오래걸리는데 기다리면은 된다

![pfsense](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbkkqvM%2FbtrOQwXedwK%2FG86GlGedPzEkpcuhY1cN21%2Fimg.png)

그럼 아래같이 뜨는데 기초적인 세팅은 모두 끝났다.

![pfsense](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbXh9kM%2FbtrO34Nphcw%2F49vW6t6BZ3MmKAWlGdMGck%2Fimg.png)

# 정말 마즈막
스냅샷은 뭔가 끝날때마다 찍어주자

![스냅샷](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbsOZBs%2FbtrOQCb4CoI%2FxMkXcm5ZbwZqD7QVRzhwyk%2Fimg.png)



![돼지](https://media.tenor.com/zlFNcbMxu5IAAAAC/good-job-kim-jong-un.gif)
