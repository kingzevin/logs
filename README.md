# logs
Working with Logs

*2018年 04月 26日 星期四 19:58:38 CST*
    modified the for.sh;
    added the zremount
    added the log.txt and log

*2018年 04月 26日 星期四 20:01:43 CST*
first trial

*2018年 04月 26日 星期四 21:50:31 CST*
installed the filebench at node17 and scped the benchmark-api to node17

*2018年 04月 26日 星期四 22:10:37 CST*
mkdir-2 completed

*2018年 04月 26日 星期四 22:55:36 CST*
mkdir-2 not finiished. we might should filter after all the clients have finished.

*2018年 04月 27日 星期五 08:38:39 CST*
Oops! ceph not installed completely after this migration!

*2018年 04月 27日 星期五 08:38:39 CST*
mkdir-2 completed!

*2018年 04月 27日 星期五 09:02:42 CST*
mkdir: 16 .f edited.

*2018年 04月 27日 星期五 10:54:36 CST*
解决了各种remount,sudo echo > log等bug,mkdir部分基本完成‘ ’

*2018年 04月 27日 星期五 15:24:18 CST*
mkdir: 32 .f edited!

*2018年 04月 27日 星期五 16:48:33 CST*
mkdir-64 completed!

*2018年 04月 27日 星期五 16:52:07 CST*
rmdir-64 .f edited!

*2018年 04月 27日 星期五 23:54:14 CST*
rmdir:64 completed!

*2018年 04月 27日 星期五 23:54:22 CST*
close:2 completed!

*2018年 04月 28日 星期六 00:37:41 CST*
create:64 edited.

*2018年 04月 28日 星期六 10:05:44 CST*
close: 64 completed!

*2018年 04月 28日 星期六 10:30:54 CST*
Bug found! we should filter after all the threads have finished.

*2018年 04月 28日 星期六 10:58:05 CST*
Little bug: in mkdir30.f.嘤嘤嘤

*2018年 04月 28日 星期六 13:13:08 CST*
mkdir: 32 completed!

*2018年 04月 28日 星期六 13:34:05 CST*
mkdir: 64 completed!

*2018年 04月 28日 星期六 14:52:35 CST*
little bug: create29.f wrong

*2018年 04月 29日 星期日 14:34:39 CST*
something wrong with create 32

*2018年 05月 31日 星期四 16:18:49 CST*
又要开始辣!测试lookup,1,2,4,8,16,32,64,128 关注top查看瓶颈

*2018年 05月 31日 星期四 16:19:04 CST*
先修改scripts

*2018年 05月 31日 星期四 16:37:17 CST*
开始配置filebench.任务确定为open.f

*2018年 05月 31日 星期四 16:38:06 CST*
发现撞完有点晕呜呜呜,大概是脑震荡要变聪明了吧

*2018年 05月 31日 星期四 18:29:34 CST*
发现open不需要filebench,等待师兄配置好ceph

*2018年 06月 02日 星期六 22:56:52 CST*
for i in `seq 5 64`; do sed "s/9/$i/g" getattr4.f > getattr${i}.f; grep $i getattr${i}.f; done

*2018年 06月 02日 星期六 22:59:18 CST*
.sh中把lookup的任务交给getattr.f去做;发现单client的时候prealloc之后的chache有点问题;filebench重新编译;发现效果不明显;调整了client cache;不明显;发现1 client的时候输出的三个latency文件的line不一样多

*2018年 06月 03日 星期日 00:46:25 CST*
问题记录:1.设置的.f的nfiles是10000,而跑完之后的statfile1有59833ops

*2018年 06月 03日 星期日 00:47:17 CST*
问题记录:2.client.log中,有很多操作有lat_ceph,没有lat_net_mds.尤其是后面

*2018年 06月 03日 星期日 00:49:42 CST*
问题记录:越到后面,net_mds之间隔的lat_ceph越多

*2018年 06月 03日 星期日 00:50:42 CST*
问题记录:就像cache都有一样,可是我的ops明明是5w+,可是为什么lat_ceph有60w+?

*2018年 06月 08日 星期五 22:24:05 CST*
for i in `seq 2 64`; do sed "s/meandirwidth=20/meandirwidth=100000/g" getattr${i}.f > getattr${i}.f; grep $i getattr${i}.f; done

*2018年 06月 08日 星期五 22:24:34 CST*
for i in `seq 2 64`; do sed "s/meandirwidth\=20/meandirwidth\=100000/g" getattr${i}.f > getattr${i}.f; grep $i getattr${i}.f; done

*2018年 06月 10日 星期日 13:15:40 CST*
发现一个问题:多client的时候filebench在prealloc之后多次调用remount,导致问题,怎么让各单位在这里保持一致呢?不能影响后面的测试,不能影像前面的prealloc

*2018年 06月 18日 星期一 01:24:14 CST*
echo $(( ( RANDOM % 3 ) + 1 ))

*2018年 07月 04日 星期三 21:09:11 CST*
发现master中sed的自动处理没有考虑nthreads=1, 以及层数的问题,(到底要不要调整meandirwidth?)
*2018年 07月 04日 星期三 21:22:45 CST*
test

*2018年 07月 04日 星期三 21:31:26 CST*
于是决定先对op.f进行手动处理,然后再sed生成.f

*2018年 07月 04日 星期三 22:34:01 CST*
我的slave.sh for lookup 初步完成

*2018年 07月 04日 星期三 22:42:41 CST*
想起来flag等还没有处理,明天跟师兄再同意一下接口

*2018年 07月 05日 星期四 12:12:12 CST*
考虑到原始的getattr.f的nthreads以及目录层数导致的meandirwidth,所以应该把sed的对象,从原始的getattr.f改成针对于我们的lookup.f

*2018年 07月 05日 星期四 12:12:39 CST*
ceph,filebench已经完成编译配置.希望有bug嘻嘻嘻

*2018年 07月 05日 星期四 12:13:04 CST*
slave.sh的工作基本完成

*2018年 07月 05日 星期四 12:13:47 CST*
对于数据处理,画图的部分应该修改之前的py,自动用plot画图,以实现自动化测试

*2018年 07月 07日 星期六 13:24:43 CST*
昂12,13,14的挂载有点问题

*2018年 07月 07日 星期六 13:24:57 CST*
[ceph@node12 mnt]$ ceph-fuse -m 10.0.0.1 /mnt/cephfs-fuse1 2018-07-07 13:23:04.908624 7f983ba8f0c0 -1 init, newargv = 0x7f98464c4ba0 newargc=9 ceph-fuse[87053]: starting ceph client 2018-07-07 13:23:04.909017 7f983ba8f0c0 -1 auth: unable to find a keyring on /etc/ceph/ceph.client.admin.keyring,/etc/ceph/ceph.keyring,/etc/ceph/keyring,/etc/ceph/keyring.bin,: (2) No such file or directory 2018-07-07 13:23:04.909038 7f983ba8f0c0 -1 monclient: ERROR: missing keyring, cannot use cephx for authentication ceph-fuse[87053]: ceph client failed with (2) No such file or directory

*2018年 07月 07日 星期六 13:25:14 CST*
ssh 转screen有问题,然后就重定向到文件了,后台执行

*2018年 07月 07日 星期六 13:25:40 CST*
ssh node$i "$ws/slave.sh $op ${clients_per_node} > /home/ceph/dfs-benchmark/results/output 2>&1 &"

2018年 7月 7日 星期六 15时19分59秒 CST
first trial - git

2018年 7月 7日 星期六 15时20分33秒 CST
second trial - git

2018年 7月 7日 星期六 15时21分20秒 CST
thrid trial - git

2018年 7月 7日 星期六 15时22分23秒 CST
fourth trial - git

