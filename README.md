# k8s-bigdata-poc

A Bigdata Stack Installation on k8s with Vagrant (Bento/Ubuntu boxes)
- 1 Master (6 Go RAM)
- 4 Nodes (12 Go RAM)
- Network: Weave
- Addons: Heapster, Influxdb, Dashboard
- Bigdata Stack: ceph, hdfs, zookeeper, spark, hadoop, storm, kafka, elasticsearch

## Poc is poc

:warning: It's a poc, not ready to use in production
* No idempotent playbook
* Bad security rules and policies
* Ceph mons dont use persistent storage, a restart of mons pods make ceph cluster unusable
* Use of no official forked version for spark and Zeppelin
* No rules or reverse-proxy for direct external access to the tools
* Need more resources CPU/RAM, and dedicated disks for each osds
* No high availability on k8s master

## The cluster

The k8s cluster installed by Ansible
![k8s cluster](https://github.com/ricofehr/k8s-bigdata-poc/raw/master/k8s-cluster.png)

## Requirements

Need lot of resources
- 54 Go RAM
- At least 4 cpu cores
- 120Go free disk space for ceph disks creation (vdi format)

Prerequisites
- Ansible for provision
- 4 folders for ceph osds disks (See Below)

## Run

```
$ sudo mkdir -p /datas/k8s/{k8s-node1,k8s-node2,k8s-node3,k8s-node4}
$ sudo chown $USER: /datas/k8s/k8s-*
$ git submodule update --init --recursive
$ vagrant up
```

Once setup done
- Add property on spark interpreter into Zeppelin Webui: 'spark.submit.deployMode' and set value to 'cluster'
- Dashboard is reached here
http://192.168.87.10:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/

## TODO
* Make playbook idempotent
* Add persistent volume for ceph mons (Currently, a restart of ceph mons make ceph down)
* Add persistent volume for elasticsearch index datas
* Add dedicated folder for hdfs persistent volumes
* Use last official docker containers for storm pods
* Ambary integration
* Add use tests for launchs some computes on spark, hadoop, storm
* Improve security policies
* Improve namespace borders
* Add an ELK for monitoring
* Add Prometheus for supervise
* Add a docker internal registry
