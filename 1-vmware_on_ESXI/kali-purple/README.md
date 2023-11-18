# Kali purple
Deffensive 관점의 운영체제 솔루션  
방어자 입장에서의 솔루션으로 기존에 ELK 패키지, arkime, suricata(snort는 포함이 안되어서 조금 아쉽다.)과 같은 보안 솔루션을 깔기 쉽게 만들어놓고 다양한 관제및 대응 도구들을 깔아놓은 컨셉의 운영체제이다.  


![kali-purple](https://cdn.shortpixel.ai/spai/w_800+q_lossless+ret_img+to_webp/https://www.stationx.net/wp-content/uploads/2023/03/6.-Recover.jpg)

# 여담
security onion도 훌륭한 솔루션이긴 하지만 상당히 복잡한 아키텍쳐를 가지고있고 커스텀하기 난이도가 높고 salt stack, OSSEC,  Playbook 솔루션이 깔려있지만 정작 사용은 하지않고 ELK와 모니터링 도구만 사용하다보니까  
계속 써야되나 했는데 마침 이렇게 kali purple이 나와서 지워버리고 갈아타기로 했다.  


ELK 설치

# hosts 
``` bash
127.0.0.1       localhost
127.0.1.1       siem my.siem
10.60.1.2       my-siem.my.siem

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

``` bash
sudo bash -c "export HOSTNAME=my-siem.my.siem; apt-get install elasticsearch -y"


sudo sed -e '/cluster.initial_master_nodes/ s/^#*/#/' -i /etc/elasticsearch/elasticsearch.yml
echo "discovery.type: single-node" | sudo tee -a /etc/elasticsearch/elasticsearch.yml


sudo apt install kibana
sudo apt install kibana
sudo /usr/share/kibana/bin/kibana-encryption-keys generate -q
# Add keys to /etc/kibana/kibana.yml


echo "server.host: \"my-siem.my.siem\"" | sudo tee -a /etc/kibana/kibana.yml
sudo systemctl enable elasticsearch kibana --now



sudo /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
sudo /usr/share/kibana/bin/kibana-verification-code


sudo /usr/share/elasticsearch/bin/elasticsearch-certutil ca
sudo /usr/share/elasticsearch/bin/elasticsearch-certutil cert --ca elastic-stack-ca.p12 --dns my-siem.my.siem,elastic.my.siem,my-siem --out kibana-server.p12
sudo openssl pkcs12 -in /usr/share/elasticsearch/elastic-stack-ca.p12 -clcerts -nokeys -out /etc/kibana/kibana-server_ca.crt
sudo openssl pkcs12 -in /usr/share/elasticsearch/kibana-server.p12 -out /etc/kibana/kibana-server.crt -clcerts -nokeys
sudo openssl pkcs12 -in /usr/share/elasticsearch/kibana-server.p12 -out /etc/kibana/kibana-server.key -nocerts -nodes
sudo chown root:kibana /etc/kibana/kibana-server_ca.crt
sudo chown root:kibana /etc/kibana/kibana-server.key
sudo chown root:kibana /etc/kibana/kibana-server.crt
sudo chmod 660 /etc/kibana/kibana-server_ca.crt
sudo chmod 660 /etc/kibana/kibana-server.key
sudo chmod 660 /etc/kibana/kibana-server.crt


echo "server.ssl.enabled: true" | sudo tee -a /etc/kibana/kibana.yml
echo "server.ssl.certificate: /etc/kibana/kibana-server.crt" | sudo tee -a /etc/kibana/kibana.yml
echo "server.ssl.key: /etc/kibana/kibana-server.key" | sudo tee -a /etc/kibana/kibana.yml
echo "server.publicBaseUrl: \"https://my-siem.my.siem:5601\"" | sudo tee -a /etc/kibana/kibana.yml


sudo /usr/share/kibana/bin/kibana-encryption-keys generate
## Copy the generated keys into /etc/kibana/kibana.yml

sudo systemctl restart kibana
```

