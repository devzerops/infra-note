[document](https://github.com/kubernetes-sigs/kubespray/blob/master/docs/nodes.md)
# node 지우기 
``` bash
ansible-playbook -i inventory/mycluster/hosts.yaml -b -K  remove-node.yml  -e "node=node20-4"
```

# node update 

``` bash
ansible-playbook -i inventory/mycluster/hosts.yaml -b -K cluster.yml -e "node=node20-2"
```

# node reset

``` bash
ansible-playbook -i inventory/mycluster/hosts.yaml -b -K reset.yml -e "node=node20-2"
```