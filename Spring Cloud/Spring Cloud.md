# Spring Cloud

## Architecture

- 服务注册中心
  - Eureka(×)
  - Zookeeper(√)
  - **Nacos**(√)
- 负载均衡
  - **Ribbon**(√)
  - LoadBalancer(√)
- 服务调用
  - Feign(×)
  - **OpenFeign**(√)
- 服务降级
  - Hystrix(×)
  - resilience4j(√)
  - **sentinel**(√)
- 服务网关
  - Zuul(×)
  - Zuul2(×)
  - **Gateway**(√)
- 服务配置
  - Config(×)
  - **Nacos**(√)
- 服务总线
  - Bus(×)
  - **Nacos**(√)