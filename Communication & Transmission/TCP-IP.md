局域网有多种技术实现，是不兼容的。互联网通过协议消除了局域网的差异，提供了统一的技术实现。
两个核心协议：
1、主机命名（IP）
2、传送机制（TCP）

IP，即Internet Protocol（因特网协议），负责联网主机之间的路由选择和寻址；
TCP，即Transmission Control Protocol（传输控制协议），负责在不可靠的传输信道之上提供可靠的抽象层。

TCP：retransmission of lost data, in-order delivery, congestion control and avoidance, data integrity
- 三次握手 3-way handshake
- 慢启动 slow-start
- 队头堵塞 Head-of-line blocking 