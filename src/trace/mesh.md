<!-- toc -->
# 网关网格系统进展跟踪

* [istio](https://istio.io/news/)
* [linkerd](https://linkerd.io/)
* [gloo](https://gloo.solo.io/)
* [kong](https://konghq.com/)

网关、网格与 [服务框架](./rpc.md) 的区别在哪里：

* 网关网格是语言无关、非植入式
* 服务框架是语言相关、植入式

服务框架基本上来说就是一个提供服务地址发现的「注册中心」，通过用户端植入的 sdk 实现更丰富的功能。植入带来耦合，维护和适配的成本非常高。通过网关或者网格实现微服务的治理是正确的发展方向。

## 实践分享


* [网易严选ServiceMesh实践](https://mp.weixin.qq.com/s/VA1a5TtkZCbcH0Q0eM3mVg)

在40并发、1600RPS的情况下，cNginx的延时增加0.4ms（相比直连），Envoy（社区版本，优化前）Client Sidecar模式延时增加0.6ms（相比直连）。

* [酷家乐的 Istio 与 Knative 实践](https://mp.weixin.qq.com/s/klCi5T6ncbK8edQLUI91Cg)

istio  与 knative  的实践，knative 是 serviceless，当前需要  Ambassador、gloo 和 istio 三者之一作为 ingress。

## 参考

1. [李佶澳的博客][1]

[1]: https://www.lijiaocn.com "李佶澳的博客"
