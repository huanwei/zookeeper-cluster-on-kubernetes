## install 

```
$ kubectl create -f nfs-client.yaml
$ kubectl create -f nfs-client-sa.yaml
$ kubectl create -f nfs-client-class.yaml

$ kubectl get pods -owide|grep nfs-client-provisioner
$ kubectl get storageclass

```

## test pvc and pod
```
$ kubectl create -f test-pvc.yaml
$ kubectl get pvc
$ kubectl get pv

$ kubectl create -f test-pod.yaml
$ kubectl get pods -owide|grep test-pod


### check on nfs server

[root@localhost ~]# ls /nfs/k8s/
default-test-pvc-pvc-9557472a-0b13-11e9-8963-005056bc2383
[root@localhost ~]# ls /nfs/k8s/default-test-pvc-pvc-9557472a-0b13-11e9-8963-005056bc2383/
SUCCESS
```

## test statefulset 
```
$ kubectl create -f test-statefulset-nfs.yaml
[root@k8s-master nfs-provisioner]# kubectl get pods -owide|grep nfs-web
nfs-web-0                                 1/1       Running       0          4m        192.168.196.63    k8s-node5
nfs-web-1                                 1/1       Running       0          3m        192.168.108.25    k8s-node3
nfs-web-2                                 1/1       Running       0          52s       192.168.122.173   k8s-node4
[root@k8s-master nfs-provisioner]# 

[root@k8s-master nfs-provisioner]# kubectl get pvc
NAME            STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS       AGE
test-pvc        Bound     pvc-9557472a-0b13-11e9-8963-005056bc2383   1Mi        RWX            demo-nfs-storage   15m
www-nfs-web-0   Bound     pvc-dd9ba937-0b14-11e9-8963-005056bc2383   1Gi        RWO            demo-nfs-storage   6m
www-nfs-web-1   Bound     pvc-ea45082d-0b14-11e9-8963-005056bc2383   1Gi        RWO            demo-nfs-storage   5m
www-nfs-web-2   Bound     pvc-4f5ca9c5-0b15-11e9-8963-005056bc2383   1Gi        RWO            demo-nfs-storage   2m

[root@k8s-master nfs-provisioner]# kubectl get pv
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS    CLAIM                   STORAGECLASS       REASON    AGE
pvc-4f5ca9c5-0b15-11e9-8963-005056bc2383   1Gi        RWO            Delete           Bound     default/www-nfs-web-2   demo-nfs-storage             2m
pvc-9557472a-0b13-11e9-8963-005056bc2383   1Mi        RWX            Delete           Bound     default/test-pvc        demo-nfs-storage             14m
pvc-dd9ba937-0b14-11e9-8963-005056bc2383   1Gi        RWO            Delete           Bound     default/www-nfs-web-0   demo-nfs-storage             5m
pvc-ea45082d-0b14-11e9-8963-005056bc2383   1Gi        RWO            Delete           Bound     default/www-nfs-web-1   demo-nfs-storage             5m

### check on nfs server

[root@localhost ~]# ls -al /nfs/k8s
total 4
drwxr-xr-x.  6 root root 4096 Dec 29 11:51 .
drwxr-xr-x. 10 root root  108 Dec 29 11:27 ..
drwxrwxrwx.  2 root root   21 Dec 29 11:43 default-test-pvc-pvc-9557472a-0b13-11e9-8963-005056bc2383
drwxrwxrwx.  2 root root    6 Dec 29 11:47 default-www-nfs-web-0-pvc-dd9ba937-0b14-11e9-8963-005056bc2383
drwxrwxrwx.  2 root root    6 Dec 29 11:48 default-www-nfs-web-1-pvc-ea45082d-0b14-11e9-8963-005056bc2383
drwxrwxrwx.  2 root root    6 Dec 29 11:51 default-www-nfs-web-2-pvc-4f5ca9c5-0b15-11e9-8963-005056bc2383

```

## note

Install nfs client on each node, to avoid "mount: wrong fs type, bad option, bad superblock ..." issue.

```
[root@k8s-node4 ~]# yum install -y nfs-utils

```