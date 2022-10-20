---
title: "资源约束"
weight: 1
# bookFlatSection: false
bookToc: true
# bookHidden: false
# bookCollapseSection: false
bookComments: true
# bookSearchExclude: false
---

# docker资源约束
## 限制CPU
- **-c 或 --cpu-shares** 在有多个容器竞争 CPU 时可以设置每个容器的 CPU 时间比例，是按比例切分CPU资源；默认权值为 1024。是一个相对的权重值。容器最终能分配到的CPU资源取决于它的cpu share占所有容器cpu share总和的比例。通过cpu share可以设置容器使用CPU的优先级。
- **--cpus** 限制容器运行的核数；从docker1.13版本之后，docker提供了--cpus参数可以限定容器能使用的CPU核数
- **--cpuset-cpus** 限制容器运行在指定的CPU核心； 运行容器运行在哪个CPU核心上，例如主机有4个CPU核心，CPU核心标识为0-3，我启动一台容器，只想让这台容器运行在标识0和3的两个CPU核心上，可以使用cpuset来指定。
- **--cpu-period、--cpu-quota** CPU周期控制，在period内，最多获得quota CPU，通过cgroups实现

## 限制内存
- **-m 或 --memory** 设置内存的使用限额，例如：100MB，2GB
- **--memory-swap** 设置内存+swap的使用限额

## 限制IO
Cgroup v1中只对Direct IO和Sync IO生效，对buffer IO不生效
- **--blkio-weight** 限制权重，默认权重为500，例如：--blkio-weight 300
- **--device-read-bps** 限制读某个设备的 bps，例如--device-read-bps /dev/sda:30MB
- **--device-write-bps** 限制写某个设备的 bps
- **--device-read-iops** 限制读某个设备的 iops
- **--device-write-iops** 限制写某个设备的 iops

# kubernetes资源约束

# lxcfs
LXCFS是一个常驻服务，它启动以后会在指定目录中自行维护与上面列出的/proc目录中的文件同名的文件，容器从lxcfs维护的/proc文件中读取数据时，得到的是容器的状态数据，而不是整个宿主机的状态。
```bash
yum install lxcfs-2.0.5-3.el7.centos.x86_64.rpm  

docker run -it -m 256m --cpus 2 --cpuset-cpus "0,1" \
      -v /var/lib/lxcfs/proc/cpuinfo:/proc/cpuinfo:rw \
      -v /var/lib/lxcfs/proc/diskstats:/proc/diskstats:rw \
      -v /var/lib/lxcfs/proc/meminfo:/proc/meminfo:rw \
      -v /var/lib/lxcfs/proc/stat:/proc/stat:rw \
      -v /var/lib/lxcfs/proc/swaps:/proc/swaps:rw \
      -v /var/lib/lxcfs/proc/uptime:/proc/uptime:rw \
      ubuntu:latest /bin/bash
```

## 在kubernetes中使用lxcfs
需要解决两个问题：

第一个问题是每个node上都要启动lxcfs，部署一个daemonset
第二个问题是将lxcfs维护的/proc文件挂载到每个容器中，阿里云用Initializers实现的做法，值得借鉴：Kubernetes之路 2 - 利用LXCFS提升容器资源可见性。
