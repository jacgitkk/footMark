在使用L2TP协议构建的VPDN典型组网中，包含LAC和LNS两部分。   

### LAC   

- LAC表示L2TP访问集中器（L2TP Access Concentrator），是附属在交换网络上的具有PPP端系统和L2TP协议处理能力的设备。

- LAC一般是一个网络接入服务器NAS，主要用于通过PSTN/ISDN网络为用户提供接入服务。

- LAC位于LNS和远端系统（远地用户和远地分支机构）之间

  > 用于在LNS和远端系统之间传递信息包，把从远端系统收到的信息包按照L2TP协议进行封装并送往LNS，将从LNS收到的信息包进行解封装并送往远端系统。

- LAC与远端系统之间可以采用本地连接或PPP链路，VPDN应用中通常为PPP链路。   

### LNS   

LNS表示L2TP网络服务器（L2TP Network Server），是PPP端系统上用于处理L2TP协议服务器端部分的设备。

> 它作为L2TP隧道的另一侧端点，是LAC的对端设备，是被LAC进行隧道传输的PPP会话的逻辑终止端点。   

### 说明   

用户――（ISDN/PSTN）――LAC――（L2TP协议）――LNS―――企业内部网络 