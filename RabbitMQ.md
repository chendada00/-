# Rabbit MQ

## 基本介绍

- 端口（5672,15672,25672）
- fanout模式
  - 一个生产者发送的消息可以被多个消费者接收（广播模式、发布订阅模式）
- direct模式（==默认==）
  - 发送消息时通过携带的key进行匹配，如果匹配不到到消费者，那么这条消息会丢失，也可以进行配置，让该消息重新进入队列

- topic模式
  - 类似direct模式，消费者进行匹配接收消息的key可以是通配符，*表示匹配一个单词，#表示匹配多个单词

- headers模式
  - 通过设置message中的header中的key，value键值对进行匹配



## 基本配置



- rabbitmq-plugins list  查看rabbitmq的管理器插件
- rabbitmq-plugins enable rabbitmq_management                安装管理控制台
- [{rabbit, [{loopback_users. []}]}].            默认只能本地登录，新增配置文件修改配置