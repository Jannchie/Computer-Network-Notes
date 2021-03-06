## 计算机网络复习笔记

### Quiz

**1. 什么是协议？**

协议是通信计算机双方必须共同遵守的一组约定。只有遵守了这个约定，计算机才能互相通信。协议的三个要素是：语法、语义、语序。

---

**2.协议分层的好处是什么？**

- 各层之间相互独立：复杂程度下降。

- 结构上可分隔开：各层都可以采用最合适的技术来实现。

- 易于实现和维护：系统已被分解为若干个相对独立的子系统。

- 灵活性好：一层发生变化其他各层不受影响。

- 能促进标准化工作：每一层的功能及所提供的服务都有精确的说明。

---

**3.五个层次的主要功能和协议是什么？**

- 应用层
    - 支持网络应用，运行网络应用程序。与传输层交互数据。
    - 包括HTTP、SMTP、DNS、FTP在内的大量协议。
- 传输层
    - 在应用程序端点之间传送应用程序报文。
    - 因特网中有TCP、UDP协议。
- 网络层
    - 将数据报从一台主机移动到另一台主机。
    - 有IP协议、RIP、OSPF、BGP等路由选择协议。
- 链路层
    - 将IP数据报通过网络发送或者从网络上接受帧再递交给网络层。
    - 有ARP协议、CSMA/CD协议，ALOHA协议。

---

**4. 因特网为什么采取层次选路？**

- 规模：
    - 越来越多的端系统与主机接入网络，使得路由器进行路由选择,计算存储与转发变得十分困难。而层次路由能够减小路由选择计算的复杂性。
- 管理自治:
    - 管理员希望按照自己的意愿运行路由器，或是希望隐藏内部组织面貌。

---

**5. AS是如何划分的？**
    
AS由相同管理下的路由器集合组成，并以此划分。

---

**6. AS包含了哪些路由选择协议？分别有什么特点？**

- RIP协议
    - 使用了DV算法的思想，用于自治系统内部。每条链路的费用为1，以跳数来决定距离。
- OSPF协议
    - 使用了LS算法的思想，用于自治系统内部。可以自定义链路的费用，支持在单个路由选择域内的层次结构，对单播和多播路由选择有着综合的支持。
- BGP协议
    - 可用于自治系统之间。包括内部的iBGP和外部eBGP。

---

**7. OSPF各种路由器的作用分别是什么？**

- 骨干路由器
    - 为AS内其他区域之间的分组提供路由选择
- 内部路由器
    - 为区域内的分组提供路由选择。
- 区域边界路由器
    - 为流向区域外的分组提供路由选择。
- 边界路由器
    - 为与自治系统外的分组提供路由选择。

---

**8. 虚电路网络和数据报网络的特点分别是什么？**

虚电路网络|数据报网络
----|----
提供连接服务|提供无连接服务
每个分组内包含虚电路标识VC号|每个分组内包含目的IP地址
一次连接所有分组以相同的路径传输|每个分组可能以不同的路径传输
故障敏感，连接中一个路由器的故障会导致所有分组无法传输|故障不敏感，路由器故障可能发生丢包，其他分组不会受到影响
分组按序到达|分组乱序到达

---

**9. TCP可靠数据传输的发送方和接收方各有什么特点？**

- TCP发送方：
    - 单一计时器：只为最早发送没有得到ACK回复的分组计时，超时只重发这一个分组。
    - 快速重传：收到三个冗余ACK直接重传分组。
    - 超时间隔加倍：每次超时后，超时的时间设置为原来的两倍。
- TCP接收方：
    - 累积确认：只发送最近得到确认的下一个分组的ACK。

---

**10. TCP接收方收到错误的包会如何处理？收到乱序的包呢？**

TCP接收方收到错误的包会发送一个冗余ACK，此ACK是对最近确认过的分组的再次确认。

如果收到乱序的包，TCO接收方的行为没有规定。根据上层应用的实现，可能选择缓存乱序的分组直到完整接收排序在之前的分组再向上递交，也可能直接丢弃。

---

**11. 为什么访问带宽远远低于实际物理带宽？**

在分组传输时存在时延以及丢失。
时延有传输时延、传播时延、排队时延、处理时延四种情况。
当传输过慢、线路异常或缓存溢出时可能发生丢包，丢包则需要重传数据。

TCP连接是有流量控制和拥塞控制，流量控制使得发送方发送的速度与接收方接收的速度能够同步，拥塞控制防止IP分组导致网络拥塞而限制发送方的发送速度。

除此之外，一些ISP也会对带宽进行限制。

---

**12. DNS服务的层次结构中各层之间的相互作用是什么？**

每一层都能提供下一层名称服务器的域名和IP地址等信息。而权威服务器能提供其接管的主机的IP地址等信息。

---

**13. 通过本地DNS手动查询 www.jannchie.xin 的解析过程是怎样的？**

假定为迭代查询。

1）本地DNS服务器首先发送DNS查询报文到根名称服务器

假定发向的根DNS服务器是 c.root-servers.net 192.33.4.12：

```
nslookup -qt=a www.jannchie.xin 192.33.4.12
```

该根名称服务器解析了.xin的部分，返回.xin顶级域名服务器域名和IP给本地DNS服务器。

假设为：a0.nic.xin 65.22.128.17

2）本地DNS服务器再向顶级域名服务器发送DNS查询报文

~~~
nslookup -qt=a www.jannchie.xin 65.22.128.17
~~~

该顶级域名服务器解析了jannchie.xin的部分，返回jannchie.xin权威服务器域名和IP给本地DNS服务器。

权威服务器和IP：dns8.hichina.com 140.205.81.24

3）本地DNS服务器再向权威服务器发送DNS查询报文

~~~
nslookup -qt=a www.jannchie.xin 140.205.81.24
~~~

如此便可以得到 www.jannchie.xin 的IP，即：101.200.42.40

---

**12. 以太网中CSMA/CD协议的工作流程是怎样的？**

1. 网络层把数据递交给适配器，适配器把数据报封装成帧，缓存在适配器中。
2. 适配器监听得知信道空闲，则开始传输帧。另一方面，如果信道正在忙，则等待，直到信道空闲。
3. 传输过程中，适配器监听信道上来自其他适配器的信号。
4. 如果适配器传输了整个帧而没有检测到来自其他适配器的信号能量，则该适配器就完成了该帧的传输。在另一方面，如果适配器在传输时检测到来自其他适配器的信号能量，则终止传输。
5. 终止传输后，适配器等待一个随机的时间量，然后返回步骤2。

---

**13. LS算法节点收集信息的过程是什么样的？**

在链路状态算法中，所有节点需要将链路状态信息传递给区域内的其他所有节点，链路信息包括与邻居节点的连通性和链路代价。从而每一个节点都拥有整个网络的拓扑结构。

---

**14. LS算法扩散的控制过程是什么样的？（洪泛控制的过程是什么样的？）**

LS算法通过链路状态广播来扩散链路状态信息。因而需要洪泛控制。主要有以下三种方法：

1. 广播序号控制：源结点将其地址以及广播序号放入广播分组，每个结点维护自己收到、存储、并转发的广播分组列表，当收到一个广播时，先检查该分组是否在广播分组列表中，如果在则不转发。
2. 反向路径转发：只有改分组到达的链路正好位于他自己返回其源的最短单播路径上时，才转发分组。
3. 生成树广播：首先在拓扑图中生成一颗最小生成树，再沿着生成树的特定链路进行广播。

---

**15. 详细描述TCP拥塞控制的过程**

TCP拥塞控制共有三个阶段，分别为慢启动，拥塞避免，快速恢复。

- 慢启动：拥塞窗口cwnd一开始设置为1个MSS，每次发送成功后拥塞窗口大小加倍，即能传送两倍数量的数据包。
- 拥塞避免阶段：直到拥塞窗口的大小大于ssthresh时进入拥塞避免阶段。在拥塞避免阶段,cwnd一次增加一个MSS，进入线性增长的阶段。如果出现了丢包，则TCP认为网络拥塞，则进入慢启动阶段，如果出现了三次冗余ACK，则进入快速重传快速恢复阶段。
- 快速恢复：如果出现了三次冗余ACK，此时需要将ssthresh设置为此时cwnd的一半，而cwnd重设为1个MSS，重新进入慢启动阶段。

---

**16. 详细描述从接入网络到获取谷歌网站页面的全过程**

1. 获取IP地址：主机生成一个DHCP发现报文，源地址为0.0.0.0，目的地址为255.255.255.255，是一个广播包。DHCP服务器收到此条报文,发送一个DHCP服务提供报文,同样通过广播形式发送，包含DHCP服务器的IP地址。主机收到了此报文，向DHCP服务器发送DHCP请求报文，然后DHCP服务器回复DHCP ACK报文。此报文包含给主机分配的IP地址。同时还包括默认网关、本地DNS服务器等信息。

2. 获取网关路由器MAC地址：主机需要解析谷歌网址获得谷歌服务器的IP，因此需要向本地DNS服务器发送DNS查询报文，经过IP地址与子网掩码按位与的操作后，发现主机与此服务器不在一个网段内，因而这条报文需要经过路由器转发，首先需要获得网关路由的MAC地址。为此，主机发送一个源IP为自身，目的IP为默认网关，源MAC为自身，目的MAC为全F的ARP查询报文，这是一个广播报文，网关收到后，向主机发送包含其MAC地址信息的ARP响应报文，主机由此获得网关的MAC地址，同时更新主机的ARP表。

3. 向本地DNS发送DNS查询报文：接着，主机发送DNS查询报文，该报文的源IP为自身，目的IP为本地DNS服务器IP，源MAC为自身，目的MAC为网关MAC地址。网关收到此报文，查阅转发表，通过RIP或OSPF或BGP协议，转发到本地DNS服务器所在的网段，最终该服务器收此查询报文。

4. DNS查询：本地DNS服务器递归或迭代依次地查询根名称服务器、顶级域名服务器、谷歌的权威DNS服务器，最终获取谷歌网站服务器的IP地址，之后发送DNS响应报文给主机。

5. 获取谷歌网站页面：主机得知了谷歌IP地址，便可以创建一个TCP套接字，向谷歌服务器发送SYN报文，谷歌服务器收到后，返回SYNACK报文，主机接着发送ACK，三次握手成立。之后便可发送HTTP请求，GET谷歌服务器上的页面，服务器收到后，返回200 OK，并传送页面数据。

---

**17. 详细描述BGP路由选择的过程**

1. 路由被指派一个本地偏好值为属性之一。本地偏好值可由路由器设置或是由学习AS内其他路由而来，由管理员决定。此时具有最高本地偏好值的路由被选择。
2. 在上条被选择的路由中，AS-PATH最短（即路径中经过的AS最少）的路由将被选择。如果该规则为唯一规则，BGP使用一种DV算法决定路径，距离测度为跳数。
3. 再上条被选择的路由中，具有最靠近NEXT-HOP的路由被选择（热土豆路由）。路由选择由AS内部算法决定。

---

**18. 分组交换网络中时延有哪些？产生的原因分别是什么？**

时延类型|产生原因
--|--
传输时延|将分组的比特推向链路而产生，与分组长度和链路传输速率有关
传播时延|取决于物理媒体的传输速率，也与链路长度有关
处理时延|检查分组首部和决定分组导向何处时产生
排队时延|链路中分组数量过大，需要在缓存中等待其他分组传输而产生
