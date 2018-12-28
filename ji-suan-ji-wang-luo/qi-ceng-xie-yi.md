**OSI**

开放系统互联\(Open System Interconnection\)。

**OSI的七层协议**

应用层：允许访问OSI环境的手段（应用协议数据单元APDU）

表示层：对数据进行翻译、加密和压缩（表示协议数据单元PPDU）

会话层：建立、管理和终止会话（会话协议数据单元SPDU）

传输层：提供端到端的可靠报文传递和错误恢复（段Segment）

网络层：负责数据包从源到宿的传递和网际互连（包PackeT）

数据链路层：将比特组装成帧和点到点的传递（帧Frame）

物理层：通过媒介传输比特,确定机械及电气规范（比特Bit）

[![](https://cloud.githubusercontent.com/assets/16357973/24079004/6dba6754-0cb8-11e7-8685-94012e703653.png "image")](https://cloud.githubusercontent.com/assets/16357973/24079004/6dba6754-0cb8-11e7-8685-94012e703653.png)

### TCP/IP 四层模型和 OSI 七层模型对应关系

|  |
| :--- |


|  | TCP/IP 四层模型 | 对应网络协议 |
| :--- | :--- | :--- |
| 应用层（Application） | 应用层 | HTTP协议（超文本传输协议）、FTP协议（文件传输协议）、TFTP协议（简单文件传输协议）、NFS（网络文件系统协议） |
| 表示层（Presentation） | 应用层 | TELNET协议（虚拟终端协议）、SNMP协议（简单网络管理协议） |
| 会话层（Session） | 应用层 | SMTP协议（简单邮件传输协议）、DNS协议\(域名解析协议\) |
| 传输层（Transport） | 传输层 | TCP（控制传输协议）、UDP（用户数据报协议） |
| 网络层（Network） | 网际互连层 | IP（网际协议）、ICMP（网际控制消息协议）、ARP（地址解析协议）、RARP（反向地址解析协议）、UUCP |
| 数据链路层（Data Link） | 网络接口层 | FDDI、Ethernet、Arpanet、PDN、SLIP（串行线路接口协议）、PPP（点对点协议） |
| 物理层（Physical） | 网络接口层 | IEEE 802.1A、IEEE 802.2到IEEE 802.11 |

TCP/IP 是一组用于实现网络互连的通信协议；该模型是首先由 ARPANET 所使用的网络体系结构。这个体系结构在它的两个主要协议出现以后被称为 TCP/IP 参考模型\(TCP/IP Reference Model\)。这一网络协议共分为四层：网络访问层、互联网层、传输层和应用层。如上表所示。  
每层的含义如下：

##### 网络接口层 ： {#网络接口层-：}

负责监视数据在主机和网络之间的交换；与OSI参考模型中的物理层和数据链路层相对应。事实上，TCP/IP本身并未定义该层的协议，而由参与互连的各网络使用自己的物理层和数据链路层协议，然后与TCP/IP的网络接入层进行连接。

##### 网际互连层 ： {#网际互连层-：}

是整个体系结构的关键部分，其功能是使主机可以把分组发往任何网络，并使分组独立地传向目标。这些分组可能经由不同的网络，到达的顺序和发送的顺序也可能不同。高层如果需要顺序收发，那么就必须自行处理对分组的排序。互联网层使用因特网协议\(IP，Internet Protocol\)。TCP/IP参考模型的互联网层和OSI参考模型的网络层在功能上非常相似。

##### 传输层\(Transport Layer\) ： {#传输层-Transport-Layer-：-1}

使源端和目的端机器上的对等实体可以进行会话。在这一层定义了两个端到端的协议：传输控制协议\(TCP，Transmission Control Protocol\)和用户数据报协议\(UDP，User Datagram Protocol\)。TCP 是面向连接的协议，它提供可靠的报文传输和对上层应用的连接服务。为此，除了基本的数据传输外，它还有可靠性保证、流量控制、多路复用、优先权和安全性控制等功能。UDP 是面向无连接的不可靠传输的协议，主要用于不需要TCP的排序和流量控制等功能的应用程序。

##### 应用层\(Application Layer\) ： {#应用层-Application-Layer-：-1}

包含所有的高层协议，包括：虚拟终端协议\(TELNET，TELecommunications NETwork\)、文件传输协议\(FTP，File Transfer Protocol\)、电子邮件传输协议\(SMTP，Simple Mail Transfer Protocol\)、域名服务\(DNS，Domain Name Service\)、网上新闻传输协议\(NNTP，Net News Transfer Protocol\)和超文本传送协议\(HTTP，HyperText Transfer Protocol\)等。TELNET 允许一台机器上的用户登录到远程机器上，并进行工作；FTP 提供有效地将文件从一台机器上移到另一台机器上的方法；SMTP 用于电子邮件的收发；DNS 用于把主机名映射到网络地址；NNTP 用于新闻的发布、检索和获取；HTTP 用于在 WWW 上获取主页。

### TCP/IP 四层模型和 OSI 七层模型区别 {#4-TCP-IP-四层模型和-OSI-七层模型区别}

共同点：

（1）OSI 参考模型和 TCP/IP 参考模型都采用了层次结构的概念。

（2）都能够提供面向连接和无连接两种通信服务机制。

不同点：

（1）OSI 采用的七层模型，而 TCP/IP 是四层结构。

（2）TCP/IP 参考模型的网络接口层实际上并没有真正的定义，只是一些概念性的描述。而 OSI 参考模型不仅分了两层，而且每一层的功能都很详尽，甚至在数据链路层又分出一个介质访问子层，专门解决局域网的共享介质问题。

（3）OSI 模型是在协议开发前设计的，具有通用性。TCP/IP 是先有协议集然后建立模型，不适用于非 TCP/IP 网络。

（4）实际应用不同，OSI 模型只是理论上的模型，并没有成熟的产品；而 TCP/IP 已经成为国际上的标准。



