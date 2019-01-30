# EasyChat

Simple message exchange network with channel-subscription based structure.

# Running EasyChat Nexus

1. Install [Rust](https://rust-lang.org/)
2. Execute `cargo run -p ease_chat_nexus` in project root

#Protocol

###基本：

 - 本协议基于WebSocket
 - 直接通过传递字符串进行信息交换，在字符串中使用`|`分割信息体。
