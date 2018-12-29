## validation

```
[root@k8s-master zookeeper]# kubectl create -f zookeeper.yaml

[root@k8s-master zookeeper]# kubectl get pods -owide|grep zk
zk-0                                  1/1       Running       0          2m        192.168.196.52    k8s-node5
zk-1                                  1/1       Running       0          2m        192.168.122.169   k8s-node4
zk-2                                  1/1       Running       0          2m        192.168.36.158    k8s-node1

[root@k8s-master zookeeper]# for i in 0 1 2; do kubectl exec zk-$i zkServer.sh status; done
ZooKeeper JMX enabled by default
Using config: /usr/bin/../etc/zookeeper/zoo.cfg
Mode: follower
ZooKeeper JMX enabled by default
Using config: /usr/bin/../etc/zookeeper/zoo.cfg
Mode: leader
ZooKeeper JMX enabled by default
Using config: /usr/bin/../etc/zookeeper/zoo.cfg
Mode: follower

[root@k8s-master zookeeper]# telnet 10.10.103.182 32181
Trying 10.10.103.182...
Connected to 10.10.103.182.
Escape character is '^]'.
^C^C^C^CConnection closed by foreign host.

```