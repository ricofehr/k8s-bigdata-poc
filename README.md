# k8s_poc

A Kubernetes Installation with Vagrant (Bento/Ubuntu boxes)
- 1 Master
- 2 Nodes
- Network: Weave
- Addons: Heapster, Influxdb, Dashboard

Need Ansible for provision
$ vagrant up

Once setup done, the dashboard is reached here
http://192.168.77.10:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
