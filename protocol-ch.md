# Protocol 协议说明

### 基本：

 - 本协议基于WebSocket
 - 直接通过传递字符串进行信息交换，在字符串中使用`|`分割信息体

### 协议种类：

pid位于数据的第一部分。一般由一个数字与一个简写字母组成。数字代表协议版本号，后方字母代表协议分类。

Client发送给Nexus的有以下类型：

 - `1h` Hello 握手
 - `1c` Channel 订阅频道
 - `1t` Transmit 发送消息

Nexus主动发送给Client的有以下类型：

- `1r` Receive 接收消息

## 协议内容：

`Tip：为了防止发生注入危险，涉及到第三方可编辑的字符串部分，都特别加入了字符串长度用于检验。`

### `1h` Hello 握手：

组合方式：1h|名称`长度(字节)`|名称

_握手是为了上报服务端，该客户端的`名称`，用于发送消息时告知其他客户端消息的来源。_

例如：`1h|12|abcdef123456`

### `1c` Channel 订阅频道：

组合方式：1c|频道名字`长度(字节)`|频道名字|需要订阅的时间(秒部分)|需要订阅的时间(纳秒部分)

_订阅就像续命，客户端需要定时订阅频道，来告知服务端需要向我发送哪些频道的消息。_

例如：
订阅c/lobby频道五分钟 `1c|7|c/lobby|300|0`

### `1t` Transmit 发送消息：

组合方式：1t|频道`长度(字节)`|频道名字|消息`长度(字节)`|消息

_客户端发送给服务端_

例如：
发送helloworld到c/lobby频道 `1t|7|c/lobby|10|helloworld`

### `1r` Receive 接收消息：

组合方式：1r|消息来源字符串`长度(字节)`|消息来源|频道字符串`长度(字节)`|频道|消息字符串`长度(字节)`|消息

_客户端订阅后，由中转枢纽主动发送给客户端。中转枢纽将忽略所有客户端发来的1r类型消息。_