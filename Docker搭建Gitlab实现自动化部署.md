# Docker搭建Gitlab实现自动化部署

**4核8G服务器**

好贵一个月，可以使用[腾讯云按量计费](https://buy.cloud.tencent.com/cvm?tab=custom&devPayMode=hourly)，用完关机。

**更新yum**

```bash
yum update
```

**安装docker**

```bash
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install docker-ce
docker -v
```

**启动docker**

```bash
systemctl start docker
# 开机启动
systemctl enable docker
```

**拉取gitlab-ce镜像**

ce为社区免费版。

```bash
docker pull gitlab/gitlab-ce
```
