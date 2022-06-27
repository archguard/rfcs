- Start Date: 2022-06-25
- RFC PR: (leave this empty)
- ArchGuard Issue: (leave this empty)

# Summary

引入 MQ 解耦源码扫描数据和分析结果数据，同时引入 MongoDB 重构后端的数据持久化层。

# Basic example

## Sourcecode Analyser -> Streaming Channel (MQ)

analyse and publish the results to MQ as events

```
val data    = sourcecode_analyser.scan      (project)
val event   = event.create                  (data)
            = streaming_channel.publish     (event)
```

## Streaming Channel (MQ) -> Follow Analyser

subscribe related topics, process further analysis

```
val event   = streaming_channel.subscribe   (topic)
val data    = following_analyser.scan       (event)
val event2  = event.create                  (data)
            = streaming_channel.publish     (event2)
```

## MQ -> Server (Mongodb)

sync and persist data into mongodb

```
val event   = streaming_channel.subscribe   (all)
            = backend.save                  (event)
```

# Motivation

## Why we need MQ

R3 多元架构模型和架构工作台的建立会依赖源码扫描的各项数据，负责的依赖关系会使前置（源码扫描/分析结果）和后继（分析结果）的界限变得更加模糊。因此我们需要一种声明式的数据依赖 —— topic，用来标准化各种数据的存取方式，并且各自独立形成弱耦合（仅依赖topic和数据模型）。

同时 MQ 还可以为 ArchGuard 提供 分布式、高可用、缓存 等能力，以提升平台化、大数据场景下的可用性。

## Why we need Mongodb

ArchGuard 将是一个“数据驱动”的架构守护工具，Mysql 并不适合这样的场景。绝大多数都是弱事务、弱类型的数据，Nosql 能提供更高的开发效率和更低的维护成本。

# Detailed design

![overall architecture](https://user-images.githubusercontent.com/24785373/175776127-a37da4b7-d0f7-4912-bde9-f880b69653aa.png)
<https://github.com/Anddd7/architecture-diagram/blob/master/archguard-r2-v1.excalidraw>

# Drawbacks

我认为必须改进现有的数据流，向流式计算进化，才能更好的支撑后续的功能。

We mush evolute to the streaming flow, if we want to go futher.

# Alternatives

(no, please comments on this)

# Adoption strategy

- New dependencies
- New modules & packages
- New deployments (docker & k8s)

# How we teach this

## phase 1 - serverv2 poc

- create a demo to push/pull data from mq (apache pulsar)
- create a new backend (serverv2) to save/get data, persist into mongodb
- create a proxy in current backend, that can save/get data from serverv2 via api
- create a sync task in mq, automatically write the data to serverv2, or direct to mongodb
- create a in-memory implementation of mq, allow lightweight execution of everything on a single node (e.g. CI)

## phase 2 - refactoring

- migrate data persistence logic to serverv2
- remove mysql and influxdb

## phase 3 - optimize (maybe a long term)

- optimize(or fix) performance of api/db in big data-set scenarios
- create k8s specs, should give resource limitation
- drop/delete unused logic

## Acceptance Criteria

- all functions work as before without mysql and influxdb
- new backend server with clearer layered architecture
- documentation of how to develop, deploy, run customized analyser

# Unresolved questions

- MQ 选型： Kafka or Pulsar
