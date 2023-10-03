# Elastic fleet and Elastic agent 

## Add Elasticsearch CA trusted fingerprint and hosts

fleet > setting > outputs > actions > host

```
Add ip fleet-server
```


fleet > setting > outputs > host > Elasticsearch CA trusted fingerprint

```
openssl x509 -in elasticsearch-ca.pem -sha256 -fingerprint | grep SHA256 | sed 's/://g'
```



## Add fleet server 

```
elastic-agent enroll \
  --fleet-server-es=https://ip-elasticsearch:9200 \
  --fleet-server-service-token=token \
  --fleet-server-policy=name-policy \
  --fleet-server-es-ca-trusted-fingerprint=ca-trusted-fingerprint
systemctl enable elastic-agent
systemctl start elastic-agent
```

## Add agent 

fleet > add agent 

```
elastic-agent enroll \
 --url=https://ip-elasticsearch:8220 \
 --enrollment-token=ca-trusted-finger \
 --insecure
systemctl enable elastic-agent
systemctl start elastic-agent
 ```

fleet > add agent > Run standalone
```
copy yaml file and past to /etc/elastic-agent/elastic-agent/yml
systemctl enable elastic-agent
systemctl start elastic-agent
```

## Use local Elastic Package Registry

Config in kibana yml
```
xpack.fleet.registryUrl: "http://127.0.0.1:8080"
```
Transfer the image to the air-gapped environment and load it:
```
docker load -i package-registry-8.8.2.tar
```
Run docker
```
docker run -it -p 8080:8080 docker.elastic.co/package-registry/distribution:8.8.2 --security-opt seccomp=unconfined
```
Install docker for centos7
```
wget http://mirror.centos.org/centos/7/extras/x86_64/Packages/container-selinux-2.107-1.el7_6.noarch.rpm
wget https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-19.03.5-3.el7.x86_64.rpm
wget https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-cli-19.03.5-3.el7.x86_64.rpm
wget https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-selinux-17.03.3.ce-1.el7.noarch.rpm
wget https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm
sudo systemctl restart network
sudo rpm -i container-selinux-2.107-1.el7_6.noarch.rpm
sudo rpm -i docker-ce-19.03.5-3.el7.x86_64.rpm docker-ce-cli-19.03.5-3.el7.x86_64.rpm docker-ce-selinux-17.03.3.ce-1.el7.noarch.rpm containerd.io-1.2.6-3.3.el7.x86_64.rpm
sudo systemctl start docker
```
[Ref local package](https://www.elastic.co/guide/en/fleet/current/air-gapped.html)  
[Ref docker](https://gist.github.com/ShockwaveNN/2e37d61fa04e19ba814667b05502bc1c)