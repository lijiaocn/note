<!-- toc -->
# 服务框架系统进展跟踪

* [sofastack](https://www.sofastack.tech/)
* [spring cloud](https://spring.io/)
* [dubbo](https://dubbo.apache.org/en-us/)
* [grpc](https://grpc.io/)
* [sentinel](https://github.com/alibaba/Sentinel)
* [goa](https://goa.design/design/overview/)
* [go-kit](https://github.com/go-kit/kit)
* [bilibili/kratos](https://github.com/bilibili/kratos)
* [nacos](https://nacos.io/zh-cn/)

非常明显，未来属于 [网格](./mesh.md)。

不过服务框架的发展依然值得关注，植入有植入的优势，虽然未来属于非植入式的，但是很多问题和方法是通用的。

主要问题：

* 服务注册信息的维护：分布式、更新及时、一致性与分区容忍
* 负载均衡策略
* 熔断限流策略
* 认证、牵引的更多策略
* 状态测度

## bilibili/kratos

[kratos](https://github.com/bilibili/kratos) 是 bilibili 研发并使用的服务框架系统。

公开分享资料：[B站在微服务治理中如何探索与实践？](https://mp.weixin.qq.com/s/OAjcQdQxQzTRRv6q52JPNg)

* 分布式 discovery 组件负责服务信息的维护
* 植入客户端，客户端执行负载均衡
* 熔断限流当前不是全局的，疑似在客户端实现

限流算法进化：

* 单机令牌桶算法：常规限速，配置不灵活
* Hystrix 熔断算法：当请求失败比率达到一定阈值之后，熔断器开启，无差别熔断
* Google SRE 弹性熔断算法：弹性熔断是根据成功率进行调整的，当成功率越高的时候，被熔断的概率就越小 
* 基于 BBR 算法的自适应限流：用 CPU\IOPS 计算出最大承载量，超出的请求入队等待

## 参考

1. [李佶澳的博客][1]

[1]: https://www.lijiaocn.com "李佶澳的博客"
