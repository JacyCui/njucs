# 计算机网络-Review02 链路层

**201220014 崔家才**



[TOC]



## 链路层服务

> 链路层提供的服务有：成帧、链路接入、可靠交付、差错检测和纠正。

### 分帧

- 发送端：
    - 将更高层的数据封装成帧，加入帧首和帧尾。
    - 增加一些错误检查和流控制的比特。
- 接收端：
    - 检查错误与流控制信息。
    - 从帧中提取数据报，传递给更高层。

### 媒介访问控制（MAC）

MAC协议规定了帧在链路上传输的规则，即结点们是如何分享信道的，如果链路的两端都只有一方，MAC会比较简单，不过如果不止一方，就需要多路访问控制了。

媒介访问控制是为了防止多个结点在同一个链路中一起说话，避免冲突。

三类技术：

- 信道划分
    - 划分信道，允许结点占用
- 轮流
    - 轮流使用信道
- 随机访问
    - 不划分信道，允许冲突
    - 但是可以从冲突中恢复（随机选择）



## 局域网

### 局域网的构成

拓扑结构











