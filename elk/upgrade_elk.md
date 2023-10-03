# Upgrade ELK to version 7.17 to 8.x

## Enable allocation shard only primary

```
PUT _cluster/settings
{
  "persistent": {
    "cluster.routing.allocation.enable": "primaries"
  }
}

```
## Upgrade with rpm

```
systemctl stop elasticsearch 
systemctl status elasticsearch 
rpm -U elasticsearch.rpm
```
## Restart Service

```
systemctl daemon-reload
systemctl start elasticsearch
systemctl status elasticsearch
```
## Enable allocation all shard 
```
PUT _cluster/settings
{
  "persistent": {
    "cluster.routing.allocation.enable": null
  }
}

```
## Check status cluster (100% to continue)

```
GET _cat/health?v=true
```
## Config change
```
elasticsearch.yml
node.master: true
node.roles: [ master ]
```
## For use ID_field

```
PUT /_cluster/settings
{
  "persistent" : {
    "indices": {
      "id_field_data": {
        "enabled": "true"
      }
    }
  }
}
```

## If elasticserch service stuck loading
```
systemctl show elasticsearch
NotifyAccess=all
```