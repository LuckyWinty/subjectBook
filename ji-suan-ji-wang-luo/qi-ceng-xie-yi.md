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

  


