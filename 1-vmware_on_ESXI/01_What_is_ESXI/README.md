# vSphere

![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbZdzmd%2FbtrOQCJOW15%2FNtgKYZliBagXpKkKw7wWHK%2Fimg.png)

* `VMware`를 활용한 [ESXI 기술](#esxi란)을 사용하는 기술 
* VMware에 내장된 `Snapshot` 기능덕분에 안정적인 서비스가 가능한 가상화 플래폼이다.
* 일반적인 가상화와는 다르게 서버의 리소스를 효율적으로 관리가 가능하다.

-----------
# ESXI란?
![img](https://www.ionos.com/digitalguide/fileadmin/_processed_/5/b/csm_bare-metal-server-en_4b9b1e239f.png)

* VMware에서 만든 가상화 OS의 종류중 하나이다.
    * 물론 일반적인 리눅스와 윈도우의 커널과 다르며 여기에서 파생된 운영체제가 아니다. 
    * VMkernel이라는 또다른 커널을 사용한 VMware 회사의 운영체제라 생각하면된다.
    * [자세한 내용](https://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/techpaper/ESXi_architecture.pdf)은 이쪽에서 확인하는걸 추천한다.
* [베어메탈 하이퍼바이저](#baremetal-arcitecture란) 아키텍쳐를 사용했다

# ESXI 이점
* 용량 활용도를 높이기 위해 `하드웨어를 통합한 방식`
* `중앙 집중식 관리`를 통해 IT 관리를 최소화 시킴
* 자본 비용과 운영 비용 절감
* 하이퍼바이저 실행에 필요한 하드웨어 리소스를 최소화함 

---------
# Baremetal Arcitecture란?

게스트 운영 체제가 하이퍼바이저 위에서 실행되고 호스트 하드웨어에서 직접 실행되며 호스트 리소스에 대한 액세스 요청을 중재하는 가상화 아키텍처
<br>
![img](https://www.ionos.com/digitalguide/fileadmin/_processed_/5/b/csm_bare-metal-server-en_4b9b1e239f.png)
* `Type1 Hypervisor` 기술을 사용
![img](https://miro.medium.com/max/1400/0*uOG3TpWM2BlBYkbg)
