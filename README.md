# elasticsearch

# Install

```console
su - root
cd /tmp`
rpm -ivh elasticsearch-7.15.2-x86_64.rpm
```

`vi /etc/passwd`
> ```sh
> elasticsearch:x:988:982:elasticsearch:/home/elasticsearch:/bin/bash

Cho phép user `elasticsearch` có thể start/stop elasticsearch service

`visudo -f /etc/sudoers`
> ```sh
> # elasticsearch
> elasticsearch ALL=(root) NOPASSWD: /bin/systemctl status elasticsearch
> elasticsearch ALL=(root) NOPASSWD: /bin/systemctl start elasticsearch
> elasticsearch ALL=(root) NOPASSWD: /bin/systemctl stop elasticsearch
> elasticsearch ALL=(root) NOPASSWD: /bin/systemctl restart elasticsearch

```console
echo "elasticsearch" >> /etc/cron.d/cron.allow
mkdir -p /home/elasticsearch
mkdir -p /data
cp /root/.bash* /home/elasticsearch
chown -R elasticsearch:elasticsearch /data
chown -R elasticsearch:elasticsearch /home/elasticsearch
chown -R elasticsearch:elasticsearch /etc/elasticsearch
```


`vi /etc/elasticsearch/elasticsearch.yml`
> ```yml
> path.data: /data
> ingest.geoip.downloader.enabled: false




Firewall config
```console
firewall-cmd --zone=public --add-port={9200/tcp,9300/tcp}
firewall-cmd --runtime-to-permanent
firewall-cmd --reload
firewall-cmd --zone=public --list-all
```

Control elasticsearch.service
```console
su - elasticsearch
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch.service
sudo systemctl start elasticsearch.service
sudo systemctl status elasticsearch.service
```

# Cluster config

`vi /etc/elasticsearch/elasticsearch.yml`
> ```sh
> cluster.name: "vnpaytest"
> node.name: "es-node-1"
> network.host: 192.168.140.176
> http.port: 9200
> discovery.zen.ping.unicast.hosts: ["192.168.140.176", "192.168.140.177", "192.168.140.178"]

`sudo systemctl restart elasticsearch.service`
