# ZooKeeper #

## 概述 ##

### 设计目标 ###

| ZooKeeper Service                                            |
| ------------------------------------------------------------ |
| ![Alt text](http://zookeeper.apache.org/doc/current/images/zkservice.jpg) |

## 入门 ##

### 脱机操作 ###

要启动ZooKeeper，你需要一个配置文件。下面是个示例，创建一个**conf/zoo.cfg**文件，内容：

```properties
tickTime=2000
dataDir=/var/lib/zookeeper
clientPort=2181
```

这个文件可以取任意名字，本文中我们叫它**conf/zoo.cfg**。改变**dataDir**的值，指定一个已经存在的目录。每个字段的含义如下：

**tickTime**

​	ZooKeeper使用毫秒作为基本时间单位。它被用作心跳，最小会话超时时长是tickTime的两倍。

**dataDir**

​	存储内存数据库快照的位置，除非另外指定，否则更新的事务日志将写到这里的数据库中。

**clientPort**

​	监听客户端连接的端口号

现在你已经创好配置文件，你可以启动ZooKeeper了：

```bash
bin/zkServer.sh start
```

### 管理ZooKeeper ###

### 连接到ZooKeeper ###

```bash
$ bin/zkCli.sh -server 127.0.0.1:2181
```

从这里，您可以尝试几个简单的命令来感受这个简单的命令行界面。首先，从发送`列表命令`开始，如`ls`：

```bash
[zk: 127.0.0.1(CONNECTED) 1] ls /
[zookeeper]
```

接下来，运行`create /zk_test my_data`创建一个新的znode，这句话创建一个新的节点并且把`"my_data"`和这个节点关联起来，如下：

```bash
[zk: 127.0.0.1(CONNECTED) 2] create /zk_test my_data
Created /zk_test
```

再发送一个`ls /`命令来看看这时目录是什么样：

```bash
[zk: 127.0.0.1(CONNECTED) 3] ls /
[zookeeper, zk_test]
```

我们注意到`zk_test`节点已经被创建了。

接下来，运行`get`命令验证和`zk_test`节点关联的数据，如：

```bash
[zk: 127.0.0.1(CONNECTED) 4] get /zk_test
my_data
cZxid = 0x2
ctime = Mon Apr 23 00:33:30 CST 2018
mZxid = 0x2
mtime = Mon Apr 23 00:33:30 CST 2018
pZxid = 0x2
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 7
numChildren = 0
```

我们可以运行`set`命令改变`zk_test`节点的关联数据，如：

```bash
[zk: 127.0.0.1(CONNECTED) 6] set /zk_test junk
cZxid = 0x2
ctime = Mon Apr 23 00:33:30 CST 2018
mZxid = 0x5
mtime = Mon Apr 23 00:45:02 CST 2018
pZxid = 0x2
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 4
numChildren = 0
[zk: 127.0.0.1(CONNECTED) 7] get /zk_test
junk
cZxid = 0x2
ctime = Mon Apr 23 00:33:30 CST 2018
mZxid = 0x5
mtime = Mon Apr 23 00:45:02 CST 2018
pZxid = 0x2
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 4
numChildren = 0
```

注意，我们在设置数据后做了一个`get`，确实，`zk_test`关联值改变了。

最后，删除这个节点，如`delete`：

```bash
[zk: 127.0.0.1(CONNECTED) 8] delete /zk_test
[zk: 127.0.0.1(CONNECTED) 9] ls /
[zookeeper]
[zk: 127.0.0.1(CONNECTED) 10] 
```



# ZooKeeper 开发者指南 #

### 使用 ZooKeeper 开发分布式应用程序 ###

## ZooKeeper 数据模型 ##

ZooKeeper 具有一个层次性的命名空间，很像一个分布式文件系统。唯一的区别是命名空间中的每个节点可以有与之关联的数据以及子节点。就像有一个文件系统那样允许一个文件也是一个文件夹。

























### ZNodes ###

