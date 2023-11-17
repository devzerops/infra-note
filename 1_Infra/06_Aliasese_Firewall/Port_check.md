# port 체크 방법

아래 방법 추천
``` bash
nmap localhost
```

비추천 방법인데 많이 알려진거

``` bash 
netstat -anp tcp | grep LISTEN
```

``` bash
sudo lsof -i -P -n | grep LISTEN
```