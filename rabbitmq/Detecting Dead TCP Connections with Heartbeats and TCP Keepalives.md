## 原址
https://www.rabbitmq.com/heartbeats.html#heartbeats-interval

## Overview

网络的失败原因很多，而且总是不易察觉(比如高丢包率)。中断的TCP链接需要一定的时间才能够被操作系统发现（在linux中，默认配置大约为11分钟）。 AMQP 0-9-1
提供了心跳的特性去报应用层可以快速的发现终端的链接（）。心跳也可以防止某些网络设备由于在一个确定时间内没有活动而可能导致的终止。

TCP keepalives是TCP栈的一个特性，具有类似的用途，非常有用(特别是在与心跳结合使用时)，但是需要内核调优，以便在大多数操作系统和发行版中都具有实用性。

## Heartbeat Timeout Value

心跳的timeout值决定了，在多久之后，对端的TCP链接应该被RabbitMQ和客户端库认为是不可达。这个值是RabbieMQ server和客户端在链接的时候通过协商得来。
客户端一定被配置成心跳请求。

默认情况下，broker和客户端会尝试去协商心跳。当两边的值均为非0时，较小的值将会被使用。如果一端使用0值（表示尝试禁用心跳）但是另外一段使用非0值，0值将会被使用。

timeout值时秒级的，默认是60秒，老发行版默认使用580秒。

## Heartbeat Frames

大约每隔2秒钟，就会发送一次心跳帧，这个值被称为心跳间隔(heartbeat interval)，在心跳丢失两次后，对端被认为是不可达。不同的客户端hhui有不同的表现，但是TCP链接将会被关闭。当一个客户端发现RabbitMQ节点是由于心跳的原因不可达，它需要去重连。

不要混淆heartbeat timeout value和heartbeat interval value是非常重要的。RabbieMQ配置暴漏了timeout value，官方支持的客户端库也是这样的。然后一些客户端可能暴漏了interval value，可能导致困惑。

任何的流量(e.g. 协议操作[protocol operations], 发送消息, 确认消息) 都会被认为是有效的心跳。客户端可能选择去发送心跳帧，无论在链接上是否有其他的流量，但有些只在必要时才进行。

## How to Disable Heartbeats

可以通过在客户端和服务器端将超时间隔设置为0来禁用心跳。
Heartbeats can be disabled by setting the timeout interval to 0 on both client and server ends.

Alternatively a very high (say, 1800 seconds) value can be used on both ends to effectively disable heartbeats as frame delivery will be too infrequent to make a practical difference.

Neither practice is recommended unless TCP keepalives are used instead with an adequately low inactivity detection period.
除非使用具有足够低的不活动检测周期的TCP保持活动，否则不推荐这两种做法。
