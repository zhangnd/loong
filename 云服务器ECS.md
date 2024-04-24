# 云服务器ECS

## 基础配置

付费模式 ：包年包月

购买数量 ：1 台

地域及可用区 ：华南1 可用区F

镜像 ：CentOS 7.9 64位（安全加固）

实例规格 ：共享标准型 s6 / ecs.s6-c1m2.small（1vCPU 2GiB）

系统盘 ：ESSD云盘 40GiB ，随实例释放，PL0（单盘IOPS性能上限1万）

## 网络和安全组

网络 ：专有网络

公网带宽 ：按固定带宽 1Mbps

##

```bash
yum update
```

```bash
yum install yum-utils
```

https://docs.docker.com/engine/install/centos

```bash
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
docker -v
docker compose version
```
