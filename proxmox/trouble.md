# Proxmox Cluster not ready – no quorum?
Proxmox에서는 node수에 따라서 아래처럼 vote수와 quorum이 존재하는데요.

quorum은 분산시스템. 즉, 클러스터에서 작업을 수행하기위해 분산트랜잭션이 획득해야하는 최소 투표수를 의미합니다.

아래  코드는 노드가 최소 1개일떄도 실행가능하다는 의미입니다.

``` bash
pvecm expected 1
```