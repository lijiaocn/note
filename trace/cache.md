<!-- toc -->
# 高速缓存系统进展跟踪

## kv 缓存

* [redis](https://redis.io/)
* [memcached](https://memcached.org/)
* [codis](https://github.com/CodisLabs/codis)
* [twitter/twemproxy](https://github.com/twitter/twemproxy)

codis 和 twemproxy 是 proxy， 前者解决了 redis 无法水平扩展的问题，后者同时解决了 redis 和 memcached 无法水平扩展的问题。这两个项目都已经多年不更新。现在 redis 支持 cluster 模式，水平扩展不再是问题。

很明显，有了 kubernetes 之后，不再需要单独维护一个巨大无比的支持多租户的缓存集群，把缓存做成 operator 直接部署在用户侧即可，不仅实现简单，还实现了天然的隔离。如果担心用户应用影响 kubernetes 集群的稳定性，可以为缓存等基础服务单独准备一个 kubernetes 集群，实现基础服务与业务系统的分离。

## redis cluster 的设计

Redis cluster 实现了两个功能：

1. 自动将数据分散到多个 redis 实例；
2. 部分 redis 实例故障时，整体依旧可用。

通过以下方式实现上述功能：

* redis 实例启动两个 tcp 端口，6379 服务于客户端，6379+10000 用于 redis 间通信
* key 在多个 redis 实例间分配，hash slot 容量为 16384（2^14），哈希算法 CRC16 
* 多 key 操作的目标必须在同一个 slot 中，写入 key 时用 hashtag 保证，{hashtag}key
* 每个 redis 实例通过 master-slave 的方式保证高可用
* redis cluster 非强一致性，数据异步同步，存在丢失的可能性（master 宕机/集群分裂等）
* redis clsuter 支持 reshard，但是不支持自动 reshard @2020-01-04 21:00:36 
* 通过手动 faileover 的方式升级

更多细节：

* [Redis cluster tutorial](https://redis.io/topics/cluster-tutorial)
* [Redis Cluster Specification](https://redis.io/topics/cluster-spec)

## 参考

1. [李佶澳的博客][1]

[1]: https://www.lijiaocn.com "李佶澳的博客"
