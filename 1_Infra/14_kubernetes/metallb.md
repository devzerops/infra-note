# metallb 
loadbalancer를 온프레미스 환경에서 쓰기위한 기술이다.  
pfsense가 있기에 된다면 bgp 프로토콜을 쓰면 좋긴한데 간단하게 L2 로드벨런싱을 사용할꺼다..(너무 오래걸릴거같음 이거 에러해결하는데도 상당히 오래걸림..)    


# ipaddress_polls.yml

``` bash
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: production
  namespace: metallb-system
spec:
  addresses:
  - 10.10.1.100-10.10.1.200

---
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: l2-advert
  namespace: metallb-system
spec:
  ipAddressPools:
  - production
```

# Test.yml

``` bash
apiVersion: v1
kind: Namespace
metadata:
  name: web
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-server
  namespace: web
spec:
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: httpd
        image: httpd:alpine
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: web-server-service
  namespace: web
spec:
  selector:
    app: web
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```

# kubectl get all -n web

``` bash
root@master20-2 /home/server/kube/test # kubectl get all -n web
NAME                              READY   STATUS    RESTARTS   AGE
pod/web-server-5879949fb7-v75hf   1/1     Running   0          4m53s

NAME                         TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
service/web-server-service   LoadBalancer   10.233.32.57   10.10.1.100   80:30557/TCP   4m53s

NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/web-server   1/1     1            1           4m54s

NAME                                    DESIRED   CURRENT   READY   AGE
replicaset.apps/web-server-5879949fb7   1         1         1       4m54s
```