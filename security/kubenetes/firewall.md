# 쿠버네티스 사용포트

| Protocol      | Direction     | Port          | Comment                                     |
| ------------- |:-------------:| -------------:|:--------------------------------------------|
| TCP	          | Inbound	      | 16443*        | Load balancer Kubernetes API server port    |
| TCP	          | Inbound	      | 6443*	        | Kubernetes API server                       |
| TCP	          | Inbound	      | 4001	        | etcd listen client port                     |
| TCP	          | Inbound	      | 2379-2380	    | etcd server client API                      |
| TCP	          | Inbound	      | 10250	        | Kubelet API                                 |
| TCP	          | Inbound	      | 10251         | kube-scheduler                              |
| TCP	          | Inbound	      | 10252	        | kube-controller-manager                     |
| TCP	          | Inbound	      | 10255	        | Read-only Kubelet API (Deprecated)          |
| TCP	          | Inbound	      | 30000-32767	  | NodePort Services                           |

