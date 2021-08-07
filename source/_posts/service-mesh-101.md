---
title: Service Mesh 101
desc: 初步了解Service Mesh相关的内容
date: 2021-08-04 11:57:36
tags:
  - 开发
  - k8s
---

目前有关于服务网格（Service Mesh）的需求日益增多，所以需要及时了解一下相关的知识。

<!--more-->

> Istio is a Service Mesh

服务网格 (Service Mesh) 管理微服务间的通信。

## 微服务架构

- business logic (BL)
- communication configurations (COMM)
- security logic (SEC)
- retry logic (R)
- metrics & tracing logic (MT)

分离业务逻辑和非业务逻辑

## Service Mesh 解决方案

### Sidecar Proxy

- handles these networking logic
- acts as a Proxy
- third-party application
- cluster operators can configure it easily

### Control Plane

- Control Plane injects the Sidecar Proxy

## 核心特性

### Traffic Splitting

蓝绿发布

### Istio 架构

Envoy proxy

Virtual Service

Destination Rule

## 参考资料

- [📺 Istio & Service Mesh - simply explained in 15 mins](https://www.youtube.com/watch?v=16fgzklcF7Y)
