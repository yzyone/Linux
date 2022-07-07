# tcpdump抓包命令 #

1、抓本级服务器所有监听端口的包：

	tcpdump -i any -XO -vvv -s0 -w /root/`hostname`_anyport.cap

2、抓本机指定端口的包（例如抓6000监听端口的包）：

	tcpdump -i any port 6000 -w /root/`hostname`_6000.cap


3、抓本机多个指定端口的包（例如抓6000和6100监听端口的包）：

	tcpdump -i any port 6000 or 6100 -w /root/`hostname`_6000_6100.cap

采用命令行方式对接口的数据包进行筛选抓取。

不带任何选项的tcpdump ，默认抓取第一个网络接口。只有将tcpdump进程终止，抓包停止。

例子

（1）

	tcpdump -i eth0 host 192.168.1.1 【过滤主机】

注：抓取所有经过网卡1，目的ip为192.168.1.1


（2）

	tcpdump -i eth0 dst port 22 【过滤端口】

注：抓取所有经过网卡1，目的端口为22的网络数据

（3）

	tcpdump -i eth0 udp 【过滤协议】

注；抓取所有经过网卡1，协议为UDP的协议。

（4）

	tcpdupmp -i lo udp 【抓取本地环路数据包】

（5）特定协议特定端口

	tcpdump udp port 22

（6）抓取特定类型的数据包

	tcpdump -i eth0 'tcp[tcpflags] = tcp-syn' 

(抓取所有经过网卡1的SYN类型的数据包)

	tcpdump -i eth0 udp dst port 53

(抓取经过网卡1的所有DNS数据包)


（7）逻辑语句过滤

	tcpdump -i eth0 '((tcp) and ((dst net 192.168.0) and (not dst host 192.168.0.2)))'

注：抓取所有经过网卡1，目的网络是192.168.0，但目的主机不是192.168.0.2的TCP数据。

（8）抓包存取

	tcpdump -i eth0 host 192.168.1.51 and port 22 -w /tmp/tcpdump.cap

注：抓取所有经过网卡1，目的主机为192.168.1.51的网络数据并存储。