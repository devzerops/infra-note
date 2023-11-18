# suricata 파일tree
``` bash
cd /etc/suricata
┌──(root㉿siem)-[/var/lib/suricata]
└─# tree
.
├── rules
│   ├── classification.config
│   └── suricata.rules
└── update
    ├── cache
    │   ├── 041abe7bde11c9e2cd7f8571d8f3d45a-emerging.rules.tar.gz
    │   ├── 1e596eb1ec1ec9b6d6121727d3d926b5-trafficid.rules
    │   └── index.yaml
    └── sources # basic list-sources 추가할떄마다 등록됨
        ├── et-open.yaml
        └── oisf-trafficid.yaml
```

# default 설정파일 
``` bash
sudo vim /etc/suricata/suricata.yaml
```

# basic rule list-source
``` bash
 sudo suricata-update list-sources
13/5/2023 -- 14:13:50 - <Info> -- Using data-directory /var/lib/suricata.
13/5/2023 -- 14:13:50 - <Info> -- Using Suricata configuration /etc/suricata/suricata.yaml
13/5/2023 -- 14:13:50 - <Info> -- Using /etc/suricata/rules for Suricata provided rules.
13/5/2023 -- 14:13:50 - <Info> -- Found Suricata version 6.0.10 at /usr/bin/suricata.
Name: et/open
  Vendor: Proofpoint
  Summary: Emerging Threats Open Ruleset
  License: MIT
Name: et/pro
  Vendor: Proofpoint
  Summary: Emerging Threats Pro Ruleset
  License: Commercial
  Replaces: et/open
  Parameters: secret-code
  Subscription: https://www.proofpoint.com/us/threat-insight/et-pro-ruleset
Name: oisf/trafficid
  Vendor: OISF
  Summary: Suricata Traffic ID ruleset
  License: MIT
Name: scwx/enhanced
  Vendor: Secureworks
  Summary: Secureworks suricata-enhanced ruleset
  License: Commercial
  Parameters: secret-code
  Subscription: https://www.secureworks.com/contact/ (Please reference CTU Countermeasures)
Name: scwx/malware
  Vendor: Secureworks
  Summary: Secureworks suricata-malware ruleset
  License: Commercial
  Parameters: secret-code
  Subscription: https://www.secureworks.com/contact/ (Please reference CTU Countermeasures)
Name: scwx/security
  Vendor: Secureworks
  Summary: Secureworks suricata-security ruleset
  License: Commercial
  Parameters: secret-code
  Subscription: https://www.secureworks.com/contact/ (Please reference CTU Countermeasures)
Name: sslbl/ssl-fp-blacklist
  Vendor: Abuse.ch
  Summary: Abuse.ch SSL Blacklist
  License: Non-Commercial
Name: sslbl/ja3-fingerprints
  Vendor: Abuse.ch
  Summary: Abuse.ch Suricata JA3 Fingerprint Ruleset
  License: Non-Commercial
Name: etnetera/aggressive
  Vendor: Etnetera a.s.
  Summary: Etnetera aggressive IP blacklist
  License: MIT
Name: tgreen/hunting
  Vendor: tgreen
  Summary: Threat hunting rules
  License: GPLv3
Name: malsilo/win-malware
  Vendor: malsilo
  Summary: Commodity malware rules
  License: MIT
Name: stamus/lateral
  Vendor: Stamus Networks
  Summary: Lateral movement rules
  License: GPL-3.0-only
```

# suricata 실행 

``` bash
systemctl enable suricata
```

``` bash
systemctl start suricata
```
