---
title: aws-ecs-guide
date: 2021-12-13 14:34:45
tags:
- aws
- ecs
- guide
categories:
- DevOps
---


## 常见问题
### ecs cluster里看不到ec2
检查ec2的启动日志，查看日志报错明细
1. 出现`Package upgrade failed`，说明网络配置问题导致无法与ecs service endpoint通信，可以采用任一方法解决
   1. 给ec2分配public ip
   2. 在VPC上配置endpoint
   3. 在subnet上配置NAT
