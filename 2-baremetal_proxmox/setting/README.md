# setting

`create port-mirror.sh script`
``` bash
vi /var/lib/vz/snippets/port-mirror.sh
```

``` bash
chmod +x /var/lib/vz/snippets/port-mirror.sh
```

``` bash
qm set 6003 --hookscript local:snippets/port-mirror.sh
```
