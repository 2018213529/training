# 20190413
这周参加了 阿里的运维工程师的面试， 很遗憾挂掉了， 这里记录一下大部分所问的问题以及部分答案。有些不懂的 查了相关相关的资料，或者道听途说一番，填上了。

上来先介绍了阿里以及职位 略过不表。。

### 网络
#### OSI
  物理层 链路层 网络层 传输层 会话层 表示层 应用层
#### TCP 如何保证有序可达
  TCP 分配一个序列号并且在一个特定的时间内等待接收主机对分配的这个序列号进行确认，若超时则重传
##### HTTP 中 Keeplive 字段
  TCP连接复用，减少TCP握手次数
#### HTTPS 中的 SSL 或 TLS 握手过程
  双方交换并确认所支持的加密套件、协议和双方的session ticket， 客户端验证服务器身份并获取其公钥， 客户端用D-H或者RSA交换第三个session ticket 最后生成一个会话密钥， 通过这个密钥使用 对称加密加密信息。

#### HTTPS 中的 TLS 或 SSL 在 OSI 中属于哪一层
  表示层

#### TCP 拥塞控制的 慢开始 和 避免拥塞
  - 慢开始 ：设置 cwnd = 1，每经过一个传输轮次 cwnd * 2
  - 避免拥塞 ： 每收到一个确认包 cwnd + 1
  - 设置一个慢开始门限ssthresh
  - 当cwnd < ssthresh 时，使用慢开始算法。
    当cwnd > ssthresh 时，改用拥塞避免算法。
    当cwnd = ssthresh 时，慢开始与拥塞避免算法任意。
  - 如果 出现丢包 则ssthresh / 2， cwnd = 1
#### DNS 一定是使用UDP吗
  否， 存在 TCP 和 DNS-Over-TLS(DOT)， TCP 可用于防止劫持（如果是路由劫持呢？？？）， 较差的网络中也还可以保证传输，Bind9 中 Master 与 Slave 的拓扑也是使用TCP保证可靠传输。
#### 广播风暴 和 网络环路
  广播风暴在二层， 由于交换机中存在回路， 广播包不断被重复发送，使得交换机负载过高，解决方案是生成树协议 STP 或者 RSTP 也可以划分 VLAN以隔离网络； 网络环路则是三层，大致是因为路由表中存在环路 或者 不可到达点未及时更新，使得数据包在两个网络中来回发送，直到TTL为0才被丢弃。


### 系统
#### 线程和进程的区别 关系，以及优势与缺势
  线程 共享内存，进程之间内存相互隔离； 一个进程至少有一个线程， 一个线程仅能属于一个进程。进程性能开销较大 利于资源的管理； 线程转换时没有上下文转换， 性能开销较低，但不利于资源的管理和保护
#### 多线程 和 多进程 优势与缺势
  多线程之间影响大，进程下的线程挂了 有可能导致主进程不稳定; 多进程间通讯使用锁或者共享内存的话 会带来更多性能开销，也不是很方便

#### KVM 和 docker 优势以及缺势
  Docker 比 KVM 要轻量化许多，可在单平台上运行多个，启动速度也优于KVM，但隔离性 和 资源限制方面 Docker 远比不过KVM

#### 子进程是否完全复制了父进程的内存
  否，子进程的代码段、数据段、堆栈都是指向父进程的物理空间，两者的虚拟空间不同，但其对应的物理空间是同一个；父子进程中有更改相应段的行为发生时，再为子进程相应的段分配物理空间（写时复制）；
  
#### 子进程 和 父进程读写同一个文件句柄，两者顺序是什么
  交替（这我哪知道。。这是面试官说的，但我实在没有找到出处）

#### 负载 和 CPU 使用率 分别代表着什么
  负载可以认为是 特定时间间隔内运行队列中的平均进程数。（emm 讲道理面试官可能自己也不知道…毕竟给我讲的时候也用了桥的那个类比）。使用率 则代表的是 单位时间内 CPU 被使用的情况，由于现在的操作系统都是给不同进程分配时间片，时间片一到便停止中断进程，所以 CPU 使用率可以看作是时间片的占用情况。


### 数据结构与算法

#### 链表 和 数组 区别 优势以及缺势
  链表的内存空间不一定是连续的，而内存的空间是连续的；在插入过程中 如果数组空间不足需要重新申请整个空间，并将原有数据拷贝过去，若是在数组头部插入还需要对每个节点进行操作，链表插入只需要申请一个节点的空间，并指针连接一下即可；但数组支持随机访问， 链表只能从头部或尾部按顺序访问

#### 哈希环 插入删除节点的流程
（我说没听过 就没问了，后来查的时候发现这玩意是见过的，只不过不知道叫哈希环。。

直接求余后哈希的话，在节点删除或者增加是会导致产生大量的迁移。哈希环实在求余计算后 将值放入顺时针方向 离最近的节点里；删除节点后， 被删除的节点的值将会落到 顺时针方向的最近节点了，增加节点也差不多。

但是有可能导致节点内容分布不平衡， 有的多有的少，这时候就引入 虚节点 这个东西， 比如原本是 N-1，N-2两个节点，引入虚节点后变成 N-1.1 N-1.2； N-2.1 N-2.2, 然后两组节点交替分布，这样就能使哈希后落入的节点相对均匀。

#### 哈希冲突
  - 开放定址法 
    - 线性探测 ： 依次判断下个单元是否为空
    - 平方探测 ： hash[i] + x^2 
    - 双散列函数 ： 使用另外的散列函数产生一个探查步长值
  - 链地址法： 把哈希值相同的构造成一个单链表
  - 再哈希法： 使用不同的函数再计算一遍
  
#### 快排 和 冒泡的时间复杂度，空间复杂度，排序时间稳定性
|   | 时间复杂度 | 空间复杂度 |
|---|---|---|
快排 | nlogn (有可能退化为冒泡) | logn (退化为冒泡为 n ) |
冒泡 | n^2 | 1

排序时间稳定性的话，，冒泡啊，不官怎样都是 n^2, 加个flag 的话可以稍稍优化

### 代码
- 快排或者冒泡 排序；
- 用锁交替输出数字和字母

就撸了个连flag都没有冒泡就跑了。。


### 总结
面试的时候回答了一半吧。。快排没考虑递归的堆栈使用然后说空间复杂度为 1 ，这应该是导致挂掉的一个很重要因素； 然后一部分东西面试官一提能想起来，比如进程之间通讯开销较大之类的，但是刚开始回答的时候完全没有玩那个方面想。这次面试也发现诸多不足，还需努力。

共勉～～