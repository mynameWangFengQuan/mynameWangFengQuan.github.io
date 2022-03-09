```xml
spring:
  application:
    name: api-gateway
  cloud:
    gateway:
      routes:
        # 路由id,可以随意写
        - id: bill-service-route
          # 代理的服务地址
          # uri: http://127.0.0.1:9091
          # 通过服务名查找
          uri: lb://bill-server
          # 路由断言，可以配置映射路径
          predicates:
            - Path=/bill/**
#          filters:
            # 表示过滤1个路径，2表示两个路径，以此类推
#            - StripPrefix=1
      # 默认过滤器，对所有路由都生效
      default-filters:
        - AddResponseHeader=X-Response-Foo, Bar
        - AddResponseHeader=abc-myname,lxs
      # 配置跨域请求，允许所有请求
      globalcors:
        corsConfigurations:
          '[/**]':
            allowedOrigins: '*' # 这种写法或者下面的都可以，*表示全部
#            allowedOrigins:
#              - "http://localhost:63342"
#              - "*"
#            allowedMethods:
#              - GET
```

- 可以通过uri: lb://bill-server这种方式使用服务名进行路由映射

- 在网关中配置跨域请求处理时，可以使用*表示允许所有请求跨域，但是注意 *应该用单引号括起来，否则会报错，运行不起来。

