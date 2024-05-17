# Docker搭建Gitlab实现自动化部署

## GitLab

### 硬件要求

文档：https://docs.gitlab.com/ee/install/requirements.html

CPU：Up to 20 Requests per Second (RPS) or 1000 users - 8 vCPU.

Memory：Up to 20 Requests per Second (RPS) or 1000 users - 8 GB (Minimum), 16 GB (Recommended).

传送门：[腾讯云按量计费](https://buy.cloud.tencent.com/cvm?tab=custom&devPayMode=hourly)

### 更新yum

```bash
yum update
```

### 安装Docker

```bash
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install docker-ce
docker -v
```

### 启动Docker

```bash
systemctl start docker
# 开机启动
systemctl enable docker
```

### 拉取镜像

ce为社区免费版。

```bash
docker pull gitlab/gitlab-ce
```

```
[root@VM-0-10-centos ~]# docker images
REPOSITORY         TAG       IMAGE ID       CREATED      SIZE
gitlab/gitlab-ce   latest    883ec00180cd   8 days ago   2.87GB
```

### 运行容器

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

### 部署成功

启动需要几分钟，请耐心等待。

浏览器访问：http://175.178.167.11

![](https://img.zhangniandong.com/2024/175.178.167.11_users_sign_in.jpg)

初始用户名：root

初始密码：

```
[root@VM-0-10-centos ~]# cat /srv/gitlab/config/initial_root_password
# WARNING: This value is valid only in the following conditions
#          1. If provided manually (either via `GITLAB_ROOT_PASSWORD` environment variable or via `gitlab_rails['initial_root_password']` setting in `gitlab.rb`, it was provided before database was seeded for the first time (usually, the first reconfigure run).
#          2. Password hasn't been changed manually, either via UI or via command line.
#
#          If the password shown here doesn't work, you must reset the admin password following https://docs.gitlab.com/ee/security/reset_user_password.html#reset-your-root-password.

Password: Ery0bdXdk6EhQLMy0Z96QRs2N2goN3Vh+jIlcDDX7WA=

# NOTE: This file will be automatically deleted in the first reconfigure run after 24 hours.
```

登录成功。

![](https://img.zhangniandong.com/2024/175.178.167.11_.jpg)
