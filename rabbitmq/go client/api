## 原地址
https://godoc.org/github.com/streadway/amqp#Channel.Publish

## func (*Channel) Publish

func (ch *Channel) Publish(exchange, key string, mandatory, immediate bool, msg Publishing) error
从客户端投递消息到服务器的交换机上。

当你想投递一条消息到一个单独的队列上，你可以使用队列名字作为routingKye投递到default交换机。因为每一个声明的队列都有一个对默认交换机的隐式路由。

因为投递是异步的，服务器将返回任何无法传递的消息。使用Channel.NotifyReturn来添加一个监听器，去处理通过调用pushlish方法，并且带有
mandatory or immediate参数为true的无法传递的消息。

当mandatory为true并且没有队列匹配上routing key时或者immediate为true并且在匹配到的队列上没有消费者准备好去接受这个消息时，消息可能是无法交付的。

当channel、connection、socker被关闭时，可能返回一个错误。错误或者缺少错误不能够表明这个服务器是否收到了投递的消息。

如果底层的socket被关闭了，但是没有挂起从内核缓冲区被刷新的数据包，那么投递的消息可能没有到达broker
使所有投递的消息到达服务器的一个简单方法是，在终止你的应用程序之前，总是调用Connection.Close方法。

确保所有投递的小弟都到达服务器的方式是添加一个listener到Channel.NotifyPublish方法，并且使用Channel.Confirm把channel的设置为confirm mode。
delivery tags和对应的confirmations从1开始。当所有的消息投递被确认后，退出。

当投递没有返回error，并且channel被设置为confirm mode，DeliveryTags的内部计数器从1开始做第一次确认。
