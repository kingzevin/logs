# 文豪 - 部署日志

## 1. 节点分配

gpu集群: 0,9,10,17,18

其中,9作为monitor,mds,进行ceph-deploy等部署,10作为client;0,17,18作为osd.

## 2. 编译安装ceph

#### 克隆源码

这里参考的是官方git,以及其他博客.

从官方git库克隆最新的mimic分支.通过

``` bash
git clone -b mimic https://github.com/ceph/ceph.git
```

#### 构建ceph

构建依赖库, 然后开始build

``` bash
./install-deps.sh # 构建依赖库

./do_cmake.sh # cmake
cd build
make
```

期间,在do_cmake.sh处产生`error:c++ not support for c++17`的错误,发现系统的`c++`连`c++14`都不支持,所以决定编译安装`gcc7.3.0`.参考[这篇csdn博客](https://blog.csdn.net/zhaojianting/article/details/81095120)安装了`gcc`到`~/usr/`下面.

然后利用新的c++执行
```
CXX=~/usr/bin/c++ ./do_cmake.sh
```
之后在`make -j`的时候发生错误:
```
-- downloading...
       src='https://dl.bintray.com/boostorg/release/1.67.0/source/boost_1_67_0.tar.bz2'
       dst='/home/zevin/ceph-src/ceph/build/boost/src/boost_1_67_0.tar.bz2'
       timeout='none'
error: downloading 'https://dl.bintray.com/boostorg/release/1.67.0/source/boost_1_67_0.tar.bz2' failed
  status_code: 1
  status_string: "Unsupported protocol"
  log:
  --- LOG BEGIN ---
  Protocol "https" not supported or disabled in libcurl
Closing connection -1

  --- LOG END ---
CMake Error at Boost-stamp/download-Boost.cmake:161 (message):
  Downloading failed
```

然后手动`wget`这个`tar.bz2`,之后发现检查到hash匹配就没有重新download了.

之后`make -j`继续报错:

``` bash
  Url 'file:///home/zevin/ceph-src/ceph/build/src/pybind/mgr/dashboard/wheelhouse' is ignored: it is neither a file nor a directory.
  Using cached https://files.pythonhosted.org/packages/67/4b/141a581104b1f6397bfa78ac9d43d8ad29a7ca43ea90a2d863fe3056e86a/six-1.11.0-py2.py3-none-any.whl
Installing collected packages: pluggy, toml, setuptools, py, six, toxvCould not install packages due to an EnvironmentError: [Errno 13] Permission denied: '/usr/lib/python3.4/site-packages/pluggy'
Consider using the `--user` option or check the permissions.

/bin/sh: /home/zevin/ceph-src/ceph/build/src/pybind/mgr/dashboard/node-env/bin/pip: No such file or directory
make[2]: *** [src/pybind/mgr/dashboard/node-env/bin/npm] Error 127
make[1]: *** [src/pybind/mgr/dashboard/CMakeFiles/mgr-dashboard-nodeenv.dir/all] Error 2
make[1]: *** Waiting for unfinished jobs....
```

我猜测是之前`./install-deps.sh`的问题.但是这个太麻烦了.先考虑用make -j1,看看是不是多线程的问题.
(这样就得花好久啊呜呜呜)   

然后`make -j1`的结果:
``` text
$ make -j1 1>../../output 2>../../outputE

$ cat ../output
...
[ 36%] Linking CXX shared library ../../../lib/libec_shec.so
[ 36%] Built target ec_shec
Scanning dependencies of target erasure_code_plugins
[ 36%] Built target erasure_code_plugins
Scanning dependencies of target ceph-mon
[ 36%] Building CXX object src/CMakeFiles/ceph-mon.dir/ceph_mon.cc.o
[ 36%] Linking CXX executable ../bin/ceph-mon

$ cat ../outputE
../lib/libmon.a(LevelDBStore.cc.o): In function `LevelDBStore::repair(std::ostream&)':
/home/zevin/ceph-src/ceph/src/kv/LevelDBStore.cc:208: undefined reference to `leveldb::RepairDB(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&, leveldb::Options const&)'
/home/zevin/ceph-src/ceph/src/kv/LevelDBStore.cc:212: undefined reference to `leveldb::Status::ToString[abi:cxx11]() const'
../lib/libmon.a(LevelDBStore.cc.o): In function `LevelDBStore::do_open(std::ostream&, bool)':
/home/zevin/ceph-src/ceph/src/kv/LevelDBStore.cc:134: undefined reference to `leveldb::DB::Open(leveldb::Options const&, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&, leveldb::DB**)'
/home/zevin/ceph-src/ceph/src/kv/LevelDBStore.cc:137: undefined reference to `leveldb::Status::ToString[abi:cxx11]() const'
../lib/libmon.a(LevelDBStore.cc.o): In function `LevelDBStore::_test_init(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&)':
/home/zevin/ceph-src/ceph/src/kv/LevelDBStore.cc:167: undefined reference to `leveldb::DB::Open(leveldb::Options const&, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&, leveldb::DB**)'
collect2: error: ld returned 1 exit status
make[2]: *** [bin/ceph-mon] Error 1
make[1]: *** [src/CMakeFiles/ceph-mon.dir/all] Error 2
make: *** [all] Error 2

```

报的错误跟之前`make -j`不一样了哎.这个报的是levelDB的错误.
之后重新跑`./install-deps.sh`之后:

``` text
$ ./install-deps.sh  1>../outputDeps 2>../outputDepsE

$ cat ../outputDepsE
./install-deps.sh: line 307: virtualenv: command not found
./install-deps.sh: line 308: /home/zevin/ceph-src/ceph/install-deps-python2.7_tmp/bin/pip: No such file or directory
./install-deps.sh: line 309: /home/zevin/ceph-src/ceph/install-deps-python2.7_tmp/bin/virtualenv: No such file or directory
./install-deps.sh: line 312: /home/zevin/ceph-src/ceph/install-deps-python2.7/bin/activate: No such file or directory
ipaserver 4.5.4 requires dbus-python, which is not installed.
ipaserver 4.5.4 requires dogtag-pki, which is not installed.
ipaserver 4.5.4 has requirement dnspython>=1.15, but you'll have dnspython 1.12.0 which is incompatible.
ipapython 4.5.4 has requirement dnspython>=1.15, but you'll have dnspython 1.12.0 which is incompatible.
Cannot uninstall 'pyparsing'. It is a distutils installed project and thus we cannot accurately determine which files belong to it which would lead to only a partial uninstall.

```

然后决定针对这里的virtualenv等一个个解决.
