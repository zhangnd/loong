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

```
[root@VM-0-10-centos ~]# docker images
REPOSITORY         TAG       IMAGE ID       CREATED      SIZE
gitlab/gitlab-ce   latest    883ec00180cd   8 days ago   2.87GB
```

**运行gitlab容器**

文档：https://docs.gitlab.com/ee/install/docker.html

```bash
mkdir -p /srv/gitlab
```

```bash
docker run --detach \
  --hostname 175.178.167.11 \
  --publish 443:443 --publish 80:80 \
  --name gitlab \
  --restart always \
  --volume /srv/gitlab/config:/etc/gitlab \
  --volume /srv/gitlab/logs:/var/log/gitlab \
  --volume /srv/gitlab/data:/var/opt/gitlab \
  --shm-size 256m \
  gitlab/gitlab-ce:latest
```
