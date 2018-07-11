# logs
Working with Logs

2018年 04月 26日 星期四 19:58:38 CST

    modified the for.sh;
    added the zremount
    added the log.txt and log

2018年 04月 26日 星期四 20:01:43 CST

    first trial

2018年 04月 26日 星期四 21:50:31 CST

    installed the filebench at node17 and scped the benchmark-api to node17

2018年 04月 26日 星期四 22:10:37 CST

    mkdir-2 completed

2018年 04月 26日 星期四 22:55:36 CST

    mkdir-2 not finiished. we might should filter after all the clients have finished.

2018年 04月 27日 星期五 08:38:39 CST

    Oops! ceph not installed completely after this migration!

2018年 04月 27日 星期五 08:38:39 CST

    mkdir-2 completed!

2018年 04月 27日 星期五 09:02:42 CST

    mkdir: 16 .f edited.

2018年 04月 27日 星期五 10:54:36 CST

    解决了各种remount,sudo echo > log等bug,mkdir部分基本完成‘ ’

2018年 04月 27日 星期五 15:24:18 CST

    mkdir: 32 .f edited!

2018年 04月 27日 星期五 16:48:33 CST

    mkdir-64 completed!

2018年 04月 27日 星期五 16:52:07 CST

    rmdir-64 .f edited!

2018年 04月 27日 星期五 23:54:14 CST

    rmdir:64 completed!

2018年 04月 27日 星期五 23:54:22 CST

    close:2 completed!

2018年 04月 28日 星期六 00:37:41 CST

    create:64 edited.

2018年 04月 28日 星期六 10:05:44 CST

    close: 64 completed!

2018年 04月 28日 星期六 10:30:54 CST

    Bug found! we should filter after all the threads have finished.

2018年 04月 28日 星期六 10:58:05 CST

    Little bug: in mkdir30.f.嘤嘤嘤

2018年 04月 28日 星期六 13:13:08 CST

    mkdir: 32 completed!

2018年 04月 28日 星期六 13:34:05 CST

    mkdir: 64 completed!

2018年 04月 28日 星期六 14:52:35 CST

    little bug: create29.f wrong

2018年 04月 29日 星期日 14:34:39 CST

    something wrong with create 32

2018年 05月 31日 星期四 16:18:49 CST

    又要开始辣!测试lookup,1,2,4,8,16,32,64,128 关注top查看瓶颈

2018年 05月 31日 星期四 16:19:04 CST

    先修改scripts

2018年 05月 31日 星期四 16:37:17 CST

    开始配置filebench.任务确定为open.f

2018年 05月 31日 星期四 16:38:06 CST

    发现撞完有点晕呜呜呜,大概是脑震荡要变聪明了吧

2018年 05月 31日 星期四 18:29:34 CST

    发现open不需要filebench,等待师兄配置好ceph

2018年 06月 02日 星期六 22:56:52 CST

    for i in `seq 5 64`; do sed "s/9/$i/g" getattr4.f > getattr${i}.f; grep $i getattr${i}.f; done

2018年 06月 02日 星期六 22:59:18 CST

    .sh中把lookup的任务交给getattr.f去做;发现单client的时候prealloc之后的chache有点问题;filebench重新编译;发现效果不明显;调整了client cache;不明显;发现1 client的时候输出的三个latency文件的line不一样多

2018年 06月 03日 星期日 00:46:25 CST

    问题记录:1.设置的.f的nfiles是10000,而跑完之后的statfile1有59833ops

2018年 06月 03日 星期日 00:47:17 CST

    问题记录:2.client.log中,有很多操作有lat_ceph,没有lat_net_mds.尤其是后面

2018年 06月 03日 星期日 00:49:42 CST

    问题记录:越到后面,net_mds之间隔的lat_ceph越多

2018年 06月 03日 星期日 00:50:42 CST

    问题记录:就像cache都有一样,可是我的ops明明是5w+,可是为什么lat_ceph有60w+?

2018年 06月 08日 星期五 22:24:05 CST

    for i in `seq 2 64`; do sed "s/meandirwidth=20/meandirwidth=100000/g" getattr${i}.f > getattr${i}.f; grep $i getattr${i}.f; done

2018年 06月 08日 星期五 22:24:34 CST

    for i in `seq 2 64`; do sed "s/meandirwidth\=20/meandirwidth\=100000/g" getattr${i}.f > getattr${i}.f; grep $i getattr${i}.f; done

2018年 06月 10日 星期日 13:15:40 CST

    发现一个问题:多client的时候filebench在prealloc之后多次调用remount,导致问题,怎么让各单位在这里保持一致呢?不能影响后面的测试,不能影像前面的prealloc

2018年 06月 18日 星期一 01:24:14 CST

    echo $(( ( RANDOM % 3 ) + 1 ))

2018年 07月 04日 星期三 21:09:11 CST

    发现master中sed的自动处理没有考虑nthreads=1, 以及层数的问题,(到底要不要调整meandirwidth?)
2018年 07月 04日 星期三 21:22:45 CST

    test

2018年 07月 04日 星期三 21:31:26 CST

    于是决定先对op.f进行手动处理,然后再sed生成.f

2018年 07月 04日 星期三 22:34:01 CST

    我的slave.sh for lookup 初步完成

2018年 07月 04日 星期三 22:42:41 CST

    想起来flag等还没有处理,明天跟师兄再同意一下接口

2018年 07月 05日 星期四 12:12:12 CST

    考虑到原始的getattr.f的nthreads以及目录层数导致的meandirwidth,所以应该把sed的对象,从原始的getattr.f改成针对于我们的lookup.f

2018年 07月 05日 星期四 12:12:39 CST

    ceph,filebench已经完成编译配置.希望有bug嘻嘻嘻

2018年 07月 05日 星期四 12:13:04 CST

    slave.sh的工作基本完成

2018年 07月 05日 星期四 12:13:47 CST

    对于数据处理,画图的部分应该修改之前的py,自动用plot画图,以实现自动化测试

2018年 07月 07日 星期六 13:24:43 CST

    昂12,13,14的挂载有点问题

2018年 07月 07日 星期六 13:24:57 CST

    [ceph@node12 mnt]$ ceph-fuse -m 10.0.0.1 /mnt/cephfs-fuse1 2018-07-07 13:23:04.908624 7f983ba8f0c0 -1 init, newargv = 0x7f98464c4ba0 newargc=9 ceph-fuse[87053]: starting ceph client 2018-07-07 13:23:04.909017 7f983ba8f0c0 -1 auth: unable to find a keyring on /etc/ceph/ceph.client.admin.keyring,/etc/ceph/ceph.keyring,/etc/ceph/keyring,/etc/ceph/keyring.bin,: (2) No such file or directory 2018-07-07 13:23:04.909038 7f983ba8f0c0 -1 monclient: ERROR: missing keyring, cannot use cephx for authentication ceph-fuse[87053]: ceph client failed with (2) No such file or directory

2018年 07月 07日 星期六 13:25:14 CST

    ssh 转screen有问题,然后就重定向到文件了,后台执行

2018年 07月 07日 星期六 13:25:40 CST

    ssh node$i "$ws/slave.sh $op ${clients_per_node} > /home/ceph/dfs-benchmark/results/output 2>&1 &"

2018年 7月 7日 星期六 15时19分59秒 CST

    first trial - git

2018年 7月 7日 星期六 15时34分53秒 CST

	撤销git commit: 
        1.git reset --hard <commit_id>; 
        2.git push origin HEAD --force;

2018年 7月 7日 星期六 17时31分52秒 CST

	提取每个client的三个阶段的latency的脚本写好, 还差最后的统计,以及画图


2018年 07月 08日 星期日 15:28:11 CST

    问题: 对于client多出来的那么多latency数据怎么处理?另外还要分析出现的原因,如果是因为client的cache(应该是了),那么为什么数量是这样的?而且我们的dirDepth明明是1,为什么会出现这些?

2018年 07月 08日 星期日 21:56:43 CST

    完成了py的统计,绘图等脚本的编写.其中,因为draw的部分因为节点没有gui的包,所以需要对一个op统计完之后scp到我的电脑下进行绘制

2018年 07月 09日 星期一 00:37:34 CST

    问题:64的mds,net都是10004行,128的都是3000行.为什么呢?

2018年 07月 09日 星期一 10:50:20 CST

    初步观察发现,数据存在偶然性,所以需要多次实验.中间的结果通过np.save的方式,以list的形式dump到磁盘上

2018年 07月 09日 星期一 10:57:47 CST

    开始跑2node的实验,分10轮,每轮开始会在dfs-benchmark/log/下留下对应轮次的文件

2018年 07月 09日 星期一 11:12:18 CST

    开始试图在py的堆叠柱状图上标数值

2018年 07月 09日 星期一 16:28:52 CST

    经过讨论,我接下来要做的事情: 1.测试过程中监测网络,最好dump下来监测数据;2.改良堆叠柱状图;3.先对1node找出瓶颈的client,然后扩展;4.初始先跑5组实验;5.按照合理的格式记录下所有的数据结果.

2018年 07月 09日 星期一 16:29:19 CST

    待解决的问题:client的cache问题

2018年 07月 10日 星期二 10:23:31 CST

    date +"%F_%H-%M"


2018年 07月 10日 星期二 10:57:26 CST

    5.合理地备份log和slave的输出,并针对体积庞大的log,单独进行tar cxf处理,压缩率有10%左右.over

2018年 07月 10日 星期二 11:05:43 CST

    为了在存到excel时,避免65536的max行数限制,所以改用openpyxl包,然后发现pip3和pip都指向了python3的pip,更改了pip的指向.然后安装了openpyxl

2018年 07月 10日 星期二 14:13:29 CST

    nethogs: 按进程查看流量占用 iptraf: 按连接/端口查看流量 ifstat: 按设备查看流量 ethtool: 诊断工具 tcpdump: 抓包工具 ss: 连接查看工具 其他: dstat, slurm, nload, bmon

2018年 07月 10日 星期二 15:24:43 CST

    ceph:1-1:1400

2018年 07月 10日 星期二 15:24:49 CST

    ceph:2-1:2800

2018年 07月 10日 星期二 15:28:25 CST

    ceph:4-1:5600

2018年 07月 10日 星期二 15:30:40 CST

    ceph:8-1:9500

2018年 07月 10日 星期二 15:35:08 CST

    ceph:16-1:15000

2018年 07月 10日 星期二 15:43:51 CST

    ceph:32-1:16300 震惊!!! 32-1 竟然是16300左右

2018年 07月 10日 星期二 15:54:29 CST

    ceph:64-1:14300 震惊!!!瓶颈了!!!没有16好

2018年 07月 10日 星期二 17:17:46 CST

    以16为分界线测试,1,2,4,8跑1node,16以上跑1,2,4node

2018年 07月 10日 星期二 20:07:25 CST

    开始5轮的16*i-i,32*i-i测试

2018年 07月 11日 星期三 00:12:45 CST

    1node的图像很诡异,明天讨论一下

2018年 07月 11日 星期三 00:13:18 CST

    花了大概7个小时,跑完了那5组

2018年 07月 11日 星期三 00:16:17 CST

    开始5组的i*64-i测试,并观测网络,以及ceph页面

2018年 07月 11日 星期三 00:19:45 CST

    256-4mds的nethogs显示,prealloc的时候mds大概20MB/s

2018年 07月 11日 星期三 00:21:03 CST

    256-4mds的nethogs显示,prealloc的时候mds大概send-20MB/s,receive-5MB/s

2018年 07月 11日 星期三 00:28:24 CST

    256-4mds的nethogs显示,running的时候mds大概send-6MB/s,receive-3MB/s

2018年 07月 11日 星期三 00:34:29 CST

    128-2mds的nethogs显示,prealloc的时候mds大概也是send-20MB/s,receive-5MB/s

2018年 07月 11日 星期三 00:34:59 CST

    其实前两个的receive确切说是6更恰当

2018年 07月 11日 星期三 00:38:22 CST

    128-2mds的nethogs显示,running的时候mds大概send-8-9MB/s,receive-4MB/s

2018年 07月 11日 星期三 00:40:12 CST

    这两个workload,ceph页面蓝色也从9000变成了11000

2018年 07月 11日 星期三 00:44:05 CST

    64-1mds的nethogs显示,prealloc的时候mds大概也是send-20MB/s,receive-5MB/s

2018年 07月 11日 星期三 00:46:30 CST

    128-2mds的nethogs显示,running的时候mds大概send-9-11MB/s,receive-4.5-5MB/s

2018年 07月 11日 星期三 00:47:03 CST

    64-1 ceph页面蓝色也从11000变成了14000

