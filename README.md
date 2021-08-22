<div align="center">
<h1>📝sky-eye</h1>
<img src="https://img.shields.io/github/license/onblog/BlogHelper"/>
<img src="https://img.shields.io/static/v1?label=electron&message=7.1.7&color="/>
<img src="https://img.shields.io/static/v1?label=mac|win|linux&message=7.1.7&color=yellow"/>
</div>

## sky-eye：实时日志报警系统

### sky-eye是什么
目前市面上的日志预警系统绝大部分都是基于队列消费的方式进行异步处理，报警多数有延迟而且不够明确，
针对这个问题，我设计了一套基于日志filter的准实时报警系统，来帮助项目开发维护人员能够更加快速的
感知项目中的各种error日志，进而保障系统稳定性。


### sky-eye 架构设计
sky-eye的主要特性：
![img.png](img/主要特性.png)
流程：
![img.png](img/img.png)

### 快速接入

###### 1、引入包
```xml
<!--log4j版本-->
<dependency>
    <groupId>com.itkevin</groupId>
    <artifactId>log4j-api</artifactId>
    <version>1.0.0</version>
</dependency>

<!--logback版本-->
<dependency>
    <groupId>com.itkevin</groupId>
    <artifactId>logback-api</artifactId>
    <version>1.0.0</version>
</dependency>
```

###### 2、设置设置启动参数
```java
// log4j
    com.itkevin.log4j.api.listener.Log4jApplicationListener
// logback
    com.itkevin.logback.api.listener.LogbackApplicationListener
// 以上两个类交由spring 管理
```
###### 3、apollo配置参数
首先需要创建对应的namespace名称为：skyeye
```properties
# 是否启动报警
skyeye.log.alarm.enabled = true
# 报警钉钉严重错误机器人配置（支持多个机器人）
skyeye.log.alarm.serious.dingtalk = [ { "webHook": "https://oapi.dingtalk.com/robot/send?access_token=xxxxx", "secret": "xxxx" } ]
# 报警钉钉机器人配置（支持多个机器人）
skyeye.log.alarm.dingtalk = [ { "webHook": "https://oapi.dingtalk.com/robot/send?access_token=xxx", "secret": "xxxx" } ]
# 堆栈行数配置
skyeye.log.alarm.stackNum = 10
# 单条报警白名单
skyeye.log.alarm.white.list = 我是白名单
# 聚合报警白名单
skyeye.log.alarm.aggre.white.list = 我是聚合白名单
# 报警间隔时间（单位分钟）
skyeye.log.alarm.notify.time = 1
# 报警次数阀值
skyeye.log.alarm.notify.count = 1
# 接口耗时报警间隔时间（单位分钟）
skyeye.log.alarm.uri.elapsed.time = 1
# 接口耗时超过阀值时间的次数阀值（阀值时间如果不指定则默认1000毫秒）
skyeye.log.alarm.uri.elapsed.count = 10
# 指定URI接口耗时时间阀值（单位毫秒，支持指定多个URI）
skyeye.log.alarm.uri.elapsed = [{"uri":"/user/logTest","elapsed":2000}]
# 指定接口耗时时间阀值（单位毫秒，全局指定，不配置默认1000毫秒）
skyeye.log.alarm.uri.elapsed.global = 1000
skyeye.log.alarm.tool = wework
```
###### 备注
- 目前接口超时报警仅支持web,后期会同步支持部分的rpc框架
- 目前仅支持apollo配置和钉钉报警，后期会开放配置接口，可自行选择配置，报警接口也会支持企业微信和飞书，并且开放通知接口，可自定接入报警


