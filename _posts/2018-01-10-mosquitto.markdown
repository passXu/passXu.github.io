---
layout: post
title:  "mosquitto!"
date:   2017-12-22 15:38:57 +0800
categories:  tls x.509 https ca rsa
catalog:    true
excerp: mosquitto mqtt
tags:
    - mqtt
    - mosquitto
---
# [mqtt协议](https://github.com/mcxiaoke/mqtt/blob/master/README.md)

- 异常连接断开发生时能通知到连接各方。
- 订阅包含一个主题过滤器（Topic Filter）和一个最大的服务质量（QoS）等级。订阅与单个会话（Session）关联。会话可以包含多于一个的订阅。会话的每个订阅都有一个不同的主题过滤器。
- 客户端和服务端之间的状态交互。一些会话持续时长与网络连接一样，另一些可以在客户端和服务端的多个连续网络连接间扩展。（会话是什么东西）
- variable header 中的keep alive，服务端在一点五倍的保持连接时间内没有收到客户端的控制报文，它必须断开客户端的网络连接，认为网络连接已断开 [MQTT-3.1.2-24]。
- will topic 和 will message ，username， password
- CleanSession 客户端和服务端可以保存会话状态，以支持跨网络连接的可靠消息传输。这个标志位用于控制会话状态的生存时间。这个标志位可以仔细研究下，保留消息不是服务端会话状态的一部分，会话终止时不能删除保留消息 [MQTT-3.1.2.7]。
- retain pub cmd中使用，如果客户端发给服务端的PUBLISH报文的保留（RETAIN）标志被设置为1，服务端必须存储这个应用消息和它的服务质量等级（QoS），以便它可以被分发给未来的主题名匹配的订阅者 [MQTT-3.3.1-5]。如果服务端收到一条保留（RETAIN）标志为1的QoS 0消息，它必须丢弃之前为那个主题保留的任何消息。它应该将这个新的QoS 0消息当作那个主题的新保留消息，但是任何时候都可以选择丢弃它 — 如果这种情况发生了，那个主题将没有保留消息 [MQTT-3.3.1-7]，有关存储状态的更多信息见 4.1节。
- 客户端重复订阅时服务器的处理是什么，客户端使用带通配符的主题过滤器请求订阅时，客户端的订阅可能会重复，因此发布的消息可能会匹配多个过滤器。对于这种情况，服务端必须将消息分发给所有订阅匹配的QoS等级最高的客户端 [MQTT-3.3.5-1]。服务端之后可以按照订阅的QoS等级，分发消息的副本给每一个匹配的订阅者。
- sub 中的qos 它给出了服务端向客户端发送应用消息所允许的最大QoS等级。
- session state
- message ordering
- retain 如果客户端发给服务端的PUBLISH报文的保留（RETAIN）标志被设置为1，服务端必须存储这个应用消息和它的服务质量等级（QoS），以便它可以被分发给未来的主题名匹配的订阅者 [MQTT-3.3.1-5]
- will

## mqtt特性

### [will](https://stackoverflow.com/questions/17270863/mqtt-what-is-the-purpose-or-usage-of-last-will-testament)

遗嘱标志 Will Flag
**位置：**连接标志的第2位。

遗嘱标志（Will Flag）被设置为1，表示如果连接请求被接受了，遗嘱（Will Message）消息必须被存储在服务端并且与这个网络连接关联。之后网络连接关闭时，服务端必须发布这个遗嘱消息，除非服务端收到DISCONNECT报文时删除了这个遗嘱消息 [MQTT-3.1.2-8] 。

遗嘱消息发布的条件，包括但不限于：

服务端检测到了一个I/O错误或者网络故障。
客户端在保持连接（Keep Alive）的时间内未能通讯。
客户端没有先发送DISCONNECT报文直接关闭了网络连接。
由于协议错误服务端关闭了网络连接。
如果遗嘱标志被设置为1，连接标志中的Will QoS和Will Retain字段会被服务端用到，同时有效载荷中必须包含Will Topic和Will Message字段 [MQTT-3.1.2-9]。

一旦被发布或者服务端收到了客户端发送的DISCONNECT报文，遗嘱消息就必须从存储的会话状态中移除 [MQTT-3.1.2-10]。

如果遗嘱标志被设置为0，连接标志中的Will QoS和Will Retain字段必须设置为0，并且有效载荷中不能包含Will Topic和Will Message字段 [MQTT-3.1.2-11]。

如果遗嘱标志被设置为0，网络连接断开时，不能发送遗嘱消息 [MQTT-3.1.2-12]。

服务端应该迅速发布遗嘱消息。在关机或故障的情况下，服务端可以推迟遗嘱消息的发布直到之后的重启。如果发生了这种情况，在服务器故障和遗嘱消息被发布之间可能会有一个延迟。

`will retain，这个flag有什么用没懂 ，好像明白了，如果will retain为1这个will message在pub的时候retain会被设置成1`

### retain

查看pub

### session state

相关flag`clean message`等，当这个flag为1时，会保存如下消息，详细查看`3.1 关于connect`

客户端的会话状态包括：

- 已经发送给服务端，但是还没有完成确认的QoS 1和QoS 2级别的消息
- 已从服务端接收，但是还没有完成确认的QoS 2级别的消息。

服务端的会话状态包括：

- 会话是否存在，即使会话状态的其它部分都是空。
- 客户端的订阅信息。
- 已经发送给客户端，但是还没有完成确认的QoS 1和QoS 2级别的消息。
- 即将传输给客户端的QoS 1和QoS 2级别的消息。
- 已从客户端接收，但是还没有完成确认的QoS 2级别的消息。
- 可选，准备发送给客户端的QoS 0级别的消息。

## mosquitto

### [mosquitto官网doc](https://mosquitto.org/documentation/)

#### [musquiito之一：mosquitto概述](https://mosquitto.org/man/mosquitto-8.html)

里面写了有关mosquitto启动参数，配置，主题的通配符，信号量（`SIGHUP`，`SIGUSR1`，`SIGUSR2`），相关文件（`配置文件`，`数据持久化文件`，`主机访问配置文件`）,服务器桥接介绍，订阅代理状态broker status，

##### mosquitto -c

##### mosquitto -d

##### mosquitto -v

##### broker status

topic marked as static are only sent once per client on subscription

`escape the dollar symbol` 转义$符号

1. the total number of bytrs received since the broker started
1. the total number of bytes send since the broker statred(好像心跳包的数据量也包含进去了，因为没有pub的时候，这个数据量也在增大)
1. the number of currently connected client(奇怪订阅之后，没有内容输出)
1. the number of disconnected persistent clients that have been expired and removed through the persistent_client_expiration option(`persistent_client_option 这个配置还不知道啥意思`)
1. the totle number of persistent clients (with clean session disableed) that are registered at the broker but are currently disconnected(`persistent client 到底是个啥`)
1. the maximum number of clients that have been connected to the broker at the same time(在cmd里面sub这个sys topic ，`ctrl c`后再sub，这个最大同时订阅数会增大)
1. the total number of active and inactive clients currently connected and registered on the broker(`除了connect ，resgistered的client 是啥意思`，然后发现cmd中订阅这个topic也会自增)
1. `$SYS/broker/connection/#` 不是很懂，与bridge有关
1. 等等 后面还有很多 sys topic
1. the number of message with QoS>0 that are awaiting ack.
1. the total number of retained message active on the broker

`persistent 和 remain 没有弄清楚`

wildcard相关`+` single hierarchy `#` final char of subscription,$sya can't match # ,$sys/# can match

##### bridges config in mosquitto.conf

##### signals

##### files

1. mosquitto.conf
1. mosquitto.db `Persistent message data storage location if persist enabled.`
1. hosts.allow,hosts.deny

#### [mosquitto之二:mosquitto.config](https://mosquitto.org/man/mosquitto-conf-5.html)

里面有关于mosquitto的各种配置

##### authentication

- require_certificate
- psk_hint
- psk_file
- use_identity_as_username

- access client control
- client id veritly
- inflight message hold size
- persistent

- [加密相关-密钥交换（密钥协商）算法及其原理](https://program-think.blogspot.com/2016/09/https-ssl-tls-3.html)

- listener
- certificate
- psk
- tsl/ssl
- bridge

#### [mosquitto之三:password](https://mosquitto.org/man/mosquitto_passwd-1.html)

管理mosquitto密码的文件

看了这个工具的用法，可是还是不知道这个密码管理工具有啥用，`好像一个file文件里面只能保存一对账号密码`

#### [mosquitto之四:tls](https://mosquitto.org/man/mosquitto-tls-7.html)

配置mosquitto的ssl/tls支持

It is important to use different certificate subject parameters for your CA, server and clients. If the certificates appear identical, even though generated separately, the broker/client will not be able to distinguish between them and you will experience difficult to diagnose errors.

###### 操作过程

1. 下载安装openssl
1. 生成证书机构证书和key ，private key = 1234，country name = zh, state or province = zj,locality name = hz ,organization name = blzn, origanizational unit name =yf , common name =xwj,email address = passfh@foxmail.com
1. 生成服务器key pass phrase for testssl\server\server.key = 12345
1. 生成一个server.csr ， challenge password = challenge 在这一步openssl crash了
1. 客户端key生成， passphrase=1234567 ，challenge password = challenge1 在这一步openssl crash了

（我认为上面的过程就是，先生成一个证书当根证书，再分别生成客户端和服务器两个证书，然后用根证书秘钥签名，然后通信的时候将签名后的证书发送给对方，由于都有根证书的公钥，就可以验证双方的身份）

#### [mosquitto之五:pub工具](https://mosquitto.org/man/mosquitto_pub-1.html)

基于mqtt 3.1/3.11版本的简单pub信息的客户端

他会发布信息并退出

命令行配置会覆盖文件配置，出了 messgae type options ，还有 -s 不知道有啥特殊`Note also that currently some options cannot be negated, e.g. -S.`

-a 没懂

psk 又是什么鬼

will register a message with the broker that will be sent out if it discnonnects unexpectedly

retain 和 will

#### [mosquitto之六：sub工具](https://mosquitto.org/man/mosquitto_sub-1.html)

基于mqtt 3.1/3.11版本的简单sub信息的客户端

#### 剩下的文档是mqtt协议和mosquitto lib介绍

#### [how to install mosquitto on window](http://www.steves-internet-guide.com/install-mosquitto-broker/)

## [Paho.mqttv3](https://www.eclipse.org/paho/clients/java/)

1. [每次调用sub，便会在程序目录生成一个`.lck`文件](https://stackoverflow.com/questions/35454528/paho-client-for-java-generating-folder-like-paho101658642587966-tcp1270011883-w)

1. [paho mqtt publish in messageArrived](https://stackoverflow.com/questions/31959203/trying-to-publish-in-messagearrived-with-the-paho-client-mqttcallback),[在messageArrived中response可能会死锁，但是为啥topic.pub就可以](https://www.eclipse.org/paho/files/javadoc/org/eclipse/paho/client/mqttv3/MqttCallback.html)

1. 疑问，自己会收到自己发送的消息

1. .lck 和 msg 文件机制