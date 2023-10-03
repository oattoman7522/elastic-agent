# Elasticsearch and Kibana version 7

## Elasticsearch

Install with rpm 

```
rpm -iv /path/to/elasticsearch
```

config elastic.yml file **/etc/elasticsearch/elasticsearch.yml**

```
network.host: "ip-elasticsearch"
```

```
systemctl enable elasticsearch
```

```
systemctl start elasticsearch
```

change password for elastic user

```
/usr/share/elasticsearch/bin/elasticsearch-reset-password -i -u elastic​
```

check status service Elasticsearch​

```
curl -kv -u elastic:P@ssw0rd -XGET https://ip-elasticsearch:9200/_cat/health?v
```

## Kibana

Install with rpm 

```
rpm -iv /path/to/kibana
```

Generate elasticsearch.serviceAccountToken for service Kibana​

```
curl -k -XPOST -u elastic:P@ssw0rd https://ip-elasticsearch:9200/_security/service/elastic/kibana/credential/token/kibana-token
```

Copy Ca-cert elastic to kibana

```
mkdir /etc/kibana/certs 
cp /etc/elasticsearch/certs/http_ca.crt /etc/kibana/certs/
```

Generate https for kibana 

```
/usr/share/elasticsearch/bin/elasticsearch-certutil cert -name kibana-server --self-signed
openssl pkcs12 -in  /etc/kibana/certs/kibana-server.p12 -out /etc/kibana/certs/kibana-cert.crt -nokeys
openssl pkcs12 -in /etc/kibana/certs/kibana-server.p12 -out /etc/kibana/certs/kibana-key.key -nodes -nocerts
```

Config kibana.yml file /etc/kibana/kibana.yml

```
server.host: "ip-kibana"​
elasticsearch.hosts: "ip-elasticsearch"
elasticsearch.serviceAccountToken: "service-AccountToken-kibana-token:"​
elasticsearch.ssl.certificateAuthorities: [ "/etc/kibana/certs/http_ca.crt" ]​
elasticsearch.ssl.verificationMode: none​
### https ###
server.ssl.enabled: true​
server.ssl.certificate: /etc/kibana/certs/kibana-cert.crt
server.ssl.key: /etc/kibana/certs/kibana-key.key
```

Start kibana

```
systemctl enable kibana
systemctl start kibana
```
# Elasticsearch and Kibana version 8

## Elasticsearch

Install with rpm 

```
rpm -iv /path/to/elasticsearch
```

config elastic.yml file **/etc/elasticsearch/elasticsearch.yml**

```
network.host: "ip-elasticsearch"
```

```
systemctl enable elasticsearch
```

```
systemctl start elasticsearch
```

change password for elastic user

```
/usr/share/elasticsearch/bin/elasticsearch-reset-password -i -u elastic​
```

check status service Elasticsearch​

```
curl -kv -u elastic:P@ssw0rd -XGET https://ip-elasticsearch:9200/_cat/health?v
```

## Kibana

Install with rpm 

```
rpm -iv /path/to/kibana
```

Generate token and Enroll Kibana

```
/usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
/usr/share/kibana/bin/kibana-setup --enrollment-token <token>
```

Copy Ca-cert elastic to kibana

```
mkdir /etc/kibana/certs 
cp /etc/elasticsearch/certs/http_ca.crt /etc/kibana/certs/
```

Generate https for kibana 

```
/usr/share/elasticsearch/bin/elasticsearch-certutil cert -name kibana-server --self-signed
openssl pkcs12 -in  /etc/kibana/certs/kibana-server.p12 -out /etc/kibana/certs/kibana-cert.crt -nokeys
openssl pkcs12 -in /etc/kibana/certs/kibana-server.p12 -out /etc/kibana/certs/kibana-key.key -nodes -nocerts
```

Config kibana.yml file /etc/kibana/kibana.yml

```
server.host: "ip-kibana"​
### https ###
server.ssl.enabled: true​
server.ssl.certificate: /etc/kibana/certs/kibana-cert.crt
server.ssl.key: /etc/kibana/certs/kibana-key.key
```

Start kibana

```
systemctl enable kibana
systemctl start kibana
```

