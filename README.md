# k8s-bigdata-poc

A Bigdata Stack Installation on k8s with Vagrant (Bento/Ubuntu boxes)
- 1 Master (6 Go RAM)
- 4 Nodes (10 Go RAM)
- Network: Weave
- Addons: Heapster, Influxdb, Dashboard

Need Ansible for provision
$ vagrant up

Once setup done, the dashboard is reached here
http://192.168.77.10:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/

Need 4 (one for each k8s node) folders for ceph osds disks
sudo mkdir -p /datas/{k8s-node1,k8s-node2,k8s-node3,k8s-node4}
