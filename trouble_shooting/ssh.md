# ssh unkown hosts 문제 해결
``` bash
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ECDSA key sent by the remote host is
SHA256:zsgQipS8vp3DiPmHC3mAuH7f9LzyZHaPJibJpeEAJV0.
Please contact your system administrator.
Add correct host key in /home/server/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /home/server/.ssh/known_hosts:13
  remove with:
  ssh-keygen -f "/home/server/.ssh/known_hosts" -R "10.60.1.2"
ECDSA host key for 10.60.1.2 has changed and you have requested strict checking.
Host key verification failed.
```

# 덮어씌우기 전용

``` bash
ssh-keyscan 10.0.0.1 >> ~/.ssh/known_hosts
```

# 파일전용

``` bash
ssh-keyscan -f host_list > ~/.ssh/known_hosts
```

# ansible용 해결법
``` bash
ansible all -m ping --list-hosts | grep -v hosts | awk '{print $1}' > host_list && \
ssh-keyscan -f host_list > ~/.ssh/known_hosts
```
