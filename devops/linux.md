# Linux



## cgroups





```
> sudo systemctl  status
● worker
    State: running
     Jobs: 0 queued
   Failed: 0 units
    Since: Sun 2021-11-07 09:50:33 UTC; 1 weeks 2 days ago
   CGroup: /
           └─system.slice
             ├─irqbalance.service
             │ └─702 /usr/sbin/irqbalance --foreground
             ├─falco.service
             │ └─27936 /usr/bin/falco --pidfile=/var/run/falco.pid
             ├─containerd.service
             │ ├─ 6484 /usr/bin/containerd
             │ ├─10652 /usr/bin/containerd-shim-runc-v2 -namespace moby -id 5530d2f97118769f263cce8565ebbf6e296ecd2ee943ffdc2870bb28fb596feb -address /run/containerd/containerd.sock
             │ ├─10693 /usr/bin/containerd-shim-runc-v2 -namespace moby -id c19c5ee3d4835e66a467ae31b4b3337c3a0570a6e003e6caaa659e9ebefb48d6 -address /run/containerd/containerd.sock
             │ ├─10703 /pause
             │ ├─10732 /pause
             │ ├─10941 /usr/bin/containerd-shim-runc-v2 -namespace moby -id a92448f0614faca098343750086593bc07905fd6e48df88b653c3faba830ff6f -address /run/containerd/containerd.sock
             │ ├─10962 /usr/local/bin/kube-proxy --config=/var/lib/kube-proxy/config.conf --hostname-override=worker
             │ ├─12010 /usr/bin/containerd-shim-runc-v2 -namespace moby -id 6a4fc68106b0495f3aea020c3871d18ecc315aa987d050b86fb3f18e1ee7e930 -address /run/containerd/containerd.sock
             │ ├─12040 /usr/local/bin/runsvdir -P /etc/service/enabled
             │ ├─12141 runsv allocate-tunnel-addrs
             │ ├─12142 runsv cni
             │ ├─12143 runsv bird
             │ ├─12144 runsv bird6
             │ ├─12145 runsv monitor-addresses
             │ ├─12146 runsv felix
             │ ├─12147 runsv confd
             │ ├─12149 calico-node -monitor-token
             │ ├─12151 calico-node -allocate-tunnel-addrs
             │ ├─12156 calico-node -monitor-addresses
             │ ├─12157 calico-node -confd
             │ ├─12158 calico-node -felix
             │ ├─12364 bird -R -s /var/run/calico/bird.ctl -d -c /etc/calico/confd/config/bird.cfg
             │ ├─12365 bird6 -R -s /var/run/calico/bird6.ctl -d -c /etc/calico/confd/config/bird6.cfg
             │ ├─18298 /usr/bin/containerd-shim-runc-v2 -namespace moby -id 4541d4cbb9d05435de1f6ee955f77a1a53ca18af00c5b0eeb1cfeb52fa25f548 -address /run/containerd/containerd.sock
             │ ├─18332 /pause
             │ ├─18468 /usr/bin/containerd-shim-runc-v2 -namespace moby -id 2ddb008b16f814a758651fdb0a97dd16ad5512ea8bebe8aa5b96d019590e8a27 -address /run/containerd/containerd.sock
             │ ├─18497 nginx: master process nginx -g daemon off;
             │ ├─18542 nginx: worker process
             │ ├─18543 nginx: worker process
             │ ├─31502 /usr/bin/containerd-shim-runc-v2 -namespace moby -id ee42c42b0c708e4b9db1c070874e76ff3ec0806928e7d737a6170479ac9d9269 -address /run/containerd/containerd.sock
             │ ├─31535 /pause
             │ ├─31673 /usr/bin/containerd-shim-runc-v2 -namespace moby -id 7fee98634e598a0f386da10f94e8e95b604029b082535939d736c26abe600c36 -address /run/containerd/containerd.sock
             │ ├─31703 nginx: master process nginx -g daemon off;
             │ ├─31755 nginx: worker process
             │ └─31756 nginx: worker process
             ├─systemd-networkd.service
             │ └─3134 /lib/systemd/systemd-networkd
             ├─docker.service
             │ └─6993 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
             ├─kubelet.service
             │ └─10323 /usr/bin/kubelet --bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf --kubeconfig=/etc/kubernetes/kubelet.conf --config=/var/lib/kubelet/config.yaml --network-plugin=cni --pod-infra-container-image=k8s.gcr

```



From above, let's see cgroups that process with ID `18497 `belongs

```
> cat /proc/18497/cgroup
12:cpuset:/kubepods/besteffort/poda8938d5b-6b18-49a4-9916-2a499ee04624/2ddb008b16f814a758651fdb0a97dd16ad5512ea8bebe8aa5b96d019590e8a27
11:memory:/kubepods/besteffort/poda8938d5b-6b18-49a4-9916-2a499ee04624/2ddb008b16f814a758651fdb0a97dd16ad5512ea8bebe8aa5b96d019590e8a27
10:freezer:/kubepods/besteffort/poda8938d5b-6b18-49a4-9916-2a499ee04624/2ddb008b16f814a758651fdb0a97dd16ad5512ea8bebe8aa5b96d019590e8a27
9:cpu,cpuacct:/kubepods/besteffort/poda8938d5b-6b18-49a4-9916-2a499ee04624/2ddb008b16f814a758651fdb0a97dd16ad5512ea8bebe8aa5b96d019590e8a27
8:rdma:/
7:hugetlb:/kubepods/besteffort/poda8938d5b-6b18-49a4-9916-2a499ee04624/2ddb008b16f814a758651fdb0a97dd16ad5512ea8bebe8aa5b96d019590e8a27
6:net_cls,net_prio:/kubepods/besteffort/poda8938d5b-6b18-49a4-9916-2a499ee04624/2ddb008b16f814a758651fdb0a97dd16ad5512ea8bebe8aa5b96d019590e8a27
5:blkio:/kubepods/besteffort/poda8938d5b-6b18-49a4-9916-2a499ee04624/2ddb008b16f814a758651fdb0a97dd16ad5512ea8bebe8aa5b96d019590e8a27
4:pids:/kubepods/besteffort/poda8938d5b-6b18-49a4-9916-2a499ee04624/2ddb008b16f814a758651fdb0a97dd16ad5512ea8bebe8aa5b96d019590e8a27
3:perf_event:/kubepods/besteffort/poda8938d5b-6b18-49a4-9916-2a499ee04624/2ddb008b16f814a758651fdb0a97dd16ad5512ea8bebe8aa5b96d019590e8a27
2:devices:/kubepods/besteffort/poda8938d5b-6b18-49a4-9916-2a499ee04624/2ddb008b16f814a758651fdb0a97dd16ad5512ea8bebe8aa5b96d019590e8a27
1:name=systemd:/kubepods/besteffort/poda8938d5b-6b18-49a4-9916-2a499ee04624/2ddb008b16f814a758651fdb0a97dd16ad5512ea8bebe8aa5b96d019590e8a27
0::/system.slice/containerd.service


```

```
cat /sys/fs/cgroup/memory/kubepods/besteffort/poda8938d5b-6b18-49a4-9916-2a499ee04624/2ddb008b16f814a758651fdb0a97dd16ad5512ea8bebe8aa5b96d019590e8a27/memory.limit_in_bytes
9223372036854771712
```
