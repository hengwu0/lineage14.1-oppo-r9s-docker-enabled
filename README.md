# lineage14.1-oppo-r9s-docker-enabled
oppo r9s rom with docker enabled

# 中文:

Goto: [https://blog.csdn.net/whgjjim/article/details/137456996](https://blog.csdn.net/whgjjim/article/details/137456996)

# English:

## 0. background

This article provides a rom compilation method for the OPPO r9s phone.

I have uploaded a rom compilation docker image (17380582683/r9s) created from the lineage14.1. You can manually modify and rebuild the rom in the docker.  
oppo r9s flash tools 1：[https://pan.baidu.com/s/1Mik8slkXpkKOctnCI5n9xg?pwd=56e9](https://pan.baidu.com/s/1Mik8slkXpkKOctnCI5n9xg?pwd=56e9)  
oppo r9s flash tools 2：[https://pan.huang1111.cn/s/b6QaHY?path=/](https://pan.huang1111.cn/s/b6QaHY?path=/)  
rom download(Please give me Star)：[https://gitub.com/hengwu0/lineage14.1-oppo-r9s-docker-enabled/releases/download/v1.0.0/lineage-14.1-20240221-UNOFFICIAL-r9s.zip](https://gitub.com/hengwu0/lineage14.1-oppo-r9s-docker-enabled/releases/download/v1.0.0/lineage-14.1-20240221-UNOFFICIAL-r9s.zip)

## 1. Environment preparation

A x64 computer with at least 80GB of free disk rom and 16GB of RAM is required, and dockerd should be installed before.

## 2. Pull Docker Image

pull command：`docker pull  17380582683/r9s`  
image size：15.3GB，size after decompressed：27.8GB  
image id：2e0246dfe168  

## 3. Create the container and then enter into it

```sh
~:/ docker  run  -itd  --name r9s  17380582683/r9s  bash
WARNING: The requested image's platform (linux/arm64/v8) does not match the detected host platform 
    (linux/amd64) and no specific platform was requested
7a74104e517e3355429f34965ca0c130a2ed8622b80ccf608db1ad7b1e46a244
~:/ docker exec -it r9s bash
```

The warning above can be ignored directly. It is because my image was uploaded in the environment of the OPPO r9s phone, so It considered to be an arm64 platform image. In fact, this image can only run on x64 machines.

## 4. Here is the modification of kernel config to support docker

tips. You can skip this step and start building the Android rom directly, the docker image have contained this modify.

```diff
diff --git a/arch/arm64/configs/r9s_defconfig b/arch/arm64/configs/r9s_defconfig
index 552b8313944..58898b6ea44 100644
--- a/arch/arm64/configs/r9s_defconfig
+++ b/arch/arm64/configs/r9s_defconfig
@@ -1,4 +1,88 @@
-CONFIG_LOCALVERSION="-Jerry"
+CONFIG_NAMESPACES=y
+CONFIG_NET_NS=y
+CONFIG_PID_NS=y
+CONFIG_IPC_NS=y
+CONFIG_UTS_NS=y
+CONFIG_CGROUPS=y
+CONFIG_CGROUP_PIDS=y
+CONFIG_CGROUP_CPUACCT=y
+CONFIG_CGROUP_DEVICE=y
+CONFIG_CGROUP_FREEZER=y
+CONFIG_CGROUP_SCHED=y
+CONFIG_CPUSETS=y
+CONFIG_MEMCG=y
+CONFIG_KEYS=y
+CONFIG_VETH=y
+CONFIG_BRIDGE=y
+CONFIG_BRIDGE_NETFILTER=y
+CONFIG_IP_NF_FILTER=y
+CONFIG_IP_NF_TARGET_MASQUERADE=y
+CONFIG_NETFILTER_XT_MATCH_ADDRTYPE=y
+CONFIG_NETFILTER_XT_MATCH_CONNTRACK=y
+CONFIG_NETFILTER_XT_MATCH_IPVS=y
+CONFIG_NETFILTER_XT_MATCH_BPF=y
+CONFIG_NETFILTER_XT_MARK=y
+CONFIG_IP_NF_NAT=y
+CONFIG_NF_NAT=y
+CONFIG_POSIX_MQUEUE=y
+CONFIG_NF_NAT_IPV4=y
+CONFIG_NF_NAT_NEEDED=y
+CONFIG_CGROUP_BPF=y
+CONFIG_USER_NS=y
+CONFIG_SECCOMP=y
+CONFIG_SECCOMP_FILTER=y
+CONFIG_CGROUP_PIDS=y
+CONFIG_MEMCG_SWAP=y
+CONFIG_MEMCG_SWAP_ENABLED=y
+CONFIG_MEMCG_KMEM=y
+CONFIG_IOSCHED_CFQ=y
+CONFIG_CFQ_GROUP_IOSCHED=y
+CONFIG_BLK_CGROUP=y
+CONFIG_BLK_DEV_THROTTLING=y
+CONFIG_CGROUP_PERF=y
+CONFIG_CGROUP_HUGETLB=y
+CONFIG_NET_CLS_CGROUP=y
+CONFIG_CGROUP_NET_PRIO=y
+CONFIG_CFS_BANDWIDTH=y
+CONFIG_FAIR_GROUP_SCHED=y
+CONFIG_RT_GROUP_SCHED=y
+CONFIG_IP_NF_TARGET_REDIRECT=y
+CONFIG_IP_VS=y
+CONFIG_IP_VS_NFCT=y
+CONFIG_IP_VS_PROTO_TCP=y
+CONFIG_IP_VS_PROTO_UDP=y
+CONFIG_IP_VS_RR=y
+CONFIG_SECURITY_SELINUX=y
+CONFIG_SECURITY_APPARMOR=y
+CONFIG_EXT4_FS=y
+CONFIG_EXT4_FS_POSIX_ACL=y
+CONFIG_EXT4_FS_SECURITY=y
+CONFIG_BRIDGE_VLAN_FILTERING=y
+CONFIG_IPVLAN=y
+CONFIG_VXLAN=y CONFIG_BRIDGE_VLAN_FILTERING=y
+CONFIG_CRYPTO=y CONFIG_CRYPTO_AEAD=y
+CONFIG_CRYPTO_GCM=y
+CONFIG_CRYPTO_SEQIV=y
+CONFIG_CRYPTO_GHASH=y CONFIG_XFRM=y
+CONFIG_XFRM_USER=y
+CONFIG_XFRM_ALGO=y
+CONFIG_INET_ESP=y
+CONFIG_INET_XFRM_MODE_TRANSPORT=y
+CONFIG_IPVLAN=y
+CONFIG_MACVLAN=y
+CONFIG_DUMMY=y
+CONFIG_NF_NAT_FTP=y
+CONFIG_NF_CONNTRACK_FTP=y
+CONFIG_NF_NAT_TFTP=y
+CONFIG_NF_CONNTRACK_TFTP=y
+CONFIG_AUFS_FS=y
+CONFIG_BTRFS_FS=y
+CONFIG_BTRFS_FS_POSIX_ACL=y
+CONFIG_BLK_DEV_DM=y
+CONFIG_DM_THIN_PROVISIONING=y
+CONFIG_OVERLAY_FS=y
+
+CONFIG_LOCALVERSION="-perf"
 CONFIG_AUDIT=y
 CONFIG_NO_HZ=y
 CONFIG_HIGH_RES_TIMERS=y
@@ -25,6 +109,7 @@ CONFIG_TASK_IO_ACCOUNTING=y
 CONFIG_OPPO_RTC_DET_SUPPORT=y
 #endif /*VENDOR_EDIT*/
 CONFIG_CGROUPS=y
+CONFIG_CGROUP_PIDS=y
 CONFIG_CGROUP_FREEZER=y
 CONFIG_CGROUP_CPUACCT=y
 CONFIG_RESOURCE_COUNTERS=y
@@ -32,8 +117,8 @@ CONFIG_CGROUP_SCHED=y
 CONFIG_RT_GROUP_SCHED=y
 CONFIG_SCHED_HMP=y
 CONFIG_NAMESPACES=y
-# CONFIG_UTS_NS is not set
-# CONFIG_PID_NS is not set
+CONFIG_UTS_NS=y
+CONFIG_PID_NS=y
 CONFIG_BLK_DEV_INITRD=y
 CONFIG_RD_BZIP2=y
 CONFIG_RD_LZMA=y
@@ -102,6 +187,7 @@ CONFIG_NET_KEY=y
 CONFIG_INET=y
 CONFIG_IP_ADVANCED_ROUTER=y
 CONFIG_IP_MULTIPLE_TABLES=y
+CONFIG_DEVPTS_MULTIPLE_INSTANCES=y
 CONFIG_IP_ROUTE_VERBOSE=y
 CONFIG_IP_PNP=y
 CONFIG_IP_PNP_DHCP=y
@@ -176,6 +262,25 @@ CONFIG_NETFILTER_XT_MATCH_STATISTIC=y
 CONFIG_NETFILTER_XT_MATCH_STRING=y
 CONFIG_NETFILTER_XT_MATCH_TIME=y
 CONFIG_NETFILTER_XT_MATCH_U32=y
+CONFIG_NETFILTER_XT_SET=y
+CONFIG_IP_SET=y
+CONFIG_IP_SET_MAX=256
+CONFIG_IP_SET_BITMAP_IP=y
+CONFIG_IP_SET_BITMAP_IPMAC=y
+CONFIG_IP_SET_BITMAP_PORT=y
+CONFIG_IP_SET_HASH_IP=y
+CONFIG_IP_SET_HASH_IPMARK=y
+CONFIG_IP_SET_HASH_IPPORT=y
+CONFIG_IP_SET_HASH_IPPORTIP=y
+CONFIG_IP_SET_HASH_IPPORTNET=y
+CONFIG_IP_SET_HASH_IPMAC=y
+CONFIG_IP_SET_HASH_MAC=y
+CONFIG_IP_SET_HASH_NETPORTNET=y
+CONFIG_IP_SET_HASH_NET=y
+CONFIG_IP_SET_HASH_NETNET=y
+CONFIG_IP_SET_HASH_NETPORT=y
+CONFIG_IP_SET_HASH_NETIFACE=y
+CONFIG_IP_SET_LIST_SET=y
 CONFIG_NF_CONNTRACK_IPV4=y
 CONFIG_IP_NF_IPTABLES=y
 CONFIG_IP_NF_MATCH_AH=y
@@ -628,6 +733,8 @@ CONFIG_MSM_TZ_LOG=y
 CONFIG_EXT2_FS=y
 CONFIG_EXT2_FS_XATTR=y
 CONFIG_EXT3_FS=y
+CONFIG_EXT3_FS_SECURITY=y
+CONFIG_EXT3_FS_POSIX_ACL=y
 # CONFIG_EXT3_DEFAULTS_TO_ORDERED is not set
 CONFIG_EXT4_FS=y
 CONFIG_EXT4_FS_SECURITY=y
@@ -755,4 +862,9 @@ CONFIG_WLAN_FEATURE_11W=y
 CONFIG_QCOM_VOWIFI_11R=y
 CONFIG_ENABLE_LINUX_REG=y
 CONFIG_WLAN_OFFLOAD_PACKETS=y
-CONFIG_QCOM_TDLS=y
\ No newline at end of file
+CONFIG_QCOM_TDLS=y
+
+CONFIG_IKCONFIG_PROC=y
+CONFIG_NAMESPACES=y
+CONFIG_PID_NS=y
+CONFIG_IPVLAN=y
diff --git a/net/ipv4/af_inet.c b/net/ipv4/af_inet.c
index 7658abf4249..f17243e4ce3 100644
--- a/net/ipv4/af_inet.c
+++ b/net/ipv4/af_inet.c
@@ -124,7 +124,8 @@
 
 static inline int current_has_network(void)
 {
-	return in_egroup_p(AID_INET) || capable(CAP_NET_RAW);
+	// return in_egroup_p(AID_INET) || capable(CAP_NET_RAW);
+	return 1;
 }
 #else
 static inline int current_has_network(void)
diff --git a/net/ipv6/af_inet6.c b/net/ipv6/af_inet6.c
index da2ced93228..9f232ebeb55 100644
--- a/net/ipv6/af_inet6.c
+++ b/net/ipv6/af_inet6.c
@@ -69,7 +69,8 @@
 
 static inline int current_has_network(void)
 {
-	return in_egroup_p(AID_INET) || capable(CAP_NET_RAW);
+	// return in_egroup_p(AID_INET) || capable(CAP_NET_RAW);
+	return 1;
 }
 #else
 static inline int current_has_network(void)
diff --git a/net/netfilter/xt_qtaguid.c b/net/netfilter/xt_qtaguid.c
index ebf383466f7..0e00952dd1f 100644
--- a/net/netfilter/xt_qtaguid.c
+++ b/net/netfilter/xt_qtaguid.c
@@ -784,7 +784,7 @@ static int iface_stat_fmt_proc_show(struct seq_file *m, void *v)
 {
 	struct proc_iface_stat_fmt_info *p = m->private;
 	struct iface_stat *iface_entry;
-	struct rtnl_link_stats64 dev_stats, *stats;
+	struct rtnl_link_stats64 *stats;
 	struct rtnl_link_stats64 no_dev_stats = {0};
 
 
@@ -792,13 +792,8 @@ static int iface_stat_fmt_proc_show(struct seq_file *m, void *v)
 		 current->pid, current->tgid, from_kuid(&init_user_ns, current_fsuid()));
 
 	iface_entry = list_entry(v, struct iface_stat, list);
+	stats = &no_dev_stats;
 
-	if (iface_entry->active) {
-		stats = dev_get_stats(iface_entry->net_dev,
-				      &dev_stats);
-	} else {
-		stats = &no_dev_stats;
-	}
 	/*
 	 * If the meaning of the data changes, then update the fmtX
 	 * string.
```

## 5. Start rom compiling

**Enter the container** and execute the following commands to build the rom.
```sh
cd  /root/r9s;
./mybuild.sh;
# Then wait for compilation
```

## 6. Flashing new compiled rom

Restart your phone and enter the TWRP. Then choose to flash in the zip rom package.

## 7. Recompile the kernel

If you want to recompile the rom again, you can delete this directory. It will clean up the previous compilation. The command is:

```sh
rm  -rf  /root/r9s/out
```

## 8. Provide root access

The compiled ROM already comes with a root enable function: "Settings -> Developer Mode -> Root" to enable it.

## 9. The final result ROM that has been modified to support docker

Considering that someone only need the final result ROM package and won't compile it. The full rom that support docker has released(Please give me Star):  
[https://github.com/hengwu0/lineage14.1-oppo-r9s-docker-enabled/releases/](https://github.com/hengwu0/lineage14.1-oppo-r9s-docker-enabled/releases/)  

## 10. Steps to install docker on your phone

Goto: https://blog.csdn.net/whgjjim/article/details/134687390 

tips. If you want to use phone as an server, your device will connect the power adapter 7\*24 hours. Which may cause battery damaged or exploded. 
But, If we handle it properly, it's like giving our server a built-in UPS. If you need support, please email me: w._heng@163.com , $50 for beer.

## 11. Thanks

Thanks to lineage team and xiaocheng20,wuxianlin,wudilsr for your open spirit.

