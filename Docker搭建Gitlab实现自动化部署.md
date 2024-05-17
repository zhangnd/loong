# Docker搭建Gitlab实现自动化部署

## GitLab

### 硬件要求

文档：https://docs.gitlab.com/ee/install/requirements.html

CPU：Up to 20 Requests per Second (RPS) or 1000 users - 8 vCPU.

Memory：Up to 20 Requests per Second (RPS) or 1000 users - 8 GB (Minimum), 16 GB (Recommended).

4核8G

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

## GitLab Runner

Install GitLab Runner on a server separate to where GitLab is installed.

将GitLab Runner与GitLab分开部署。

在另一台服务器上部署GitLab Runner，配置采用2核4G。

### 拉取镜像

```bash
docker pull gitlab/gitlab-runner
```

```
[root@VM-0-10-centos ~]# docker images
REPOSITORY             TAG       IMAGE ID       CREATED       SIZE
gitlab/gitlab-ce       latest    883ec00180cd   8 days ago    2.87GB
gitlab/gitlab-runner   latest    1d176dab5774   12 days ago   766MB
```

### 运行容器

文档：https://docs.gitlab.com/runner/install/docker.html

```bash
mkdir -p /srv/gitlab-runner/config
```

```bash
docker run -d --name gitlab-runner --restart always \
  -v /srv/gitlab-runner/config:/etc/gitlab-runner \
  -v /var/run/docker.sock:/var/run/docker.sock \
  gitlab/gitlab-runner:latest
```

### 创建runner

文档：https://docs.gitlab.com/ee/ci/runners/runners_scope.html

打开：http://175.178.167.11/admin/runners

点击New instance runner。

![](https://img.zhangniandong.com/2024/175.178.167.11_admin_runners.jpg)

填写tag，点击Create runner。

![](https://img.zhangniandong.com/2024/175.178.167.11_admin_runners_new.jpg)

创建成功，得到token：glrt-5AZqiUy5yypqwLuhGrxz

### 注册runner

文档：https://docs.gitlab.com/runner/register/index.html

```bash
docker run --rm -v /srv/gitlab-runner/config:/etc/gitlab-runner gitlab/gitlab-runner register \
  --non-interactive \
  --url "http://175.178.167.11" \
  --token "glrt-5AZqiUy5yypqwLuhGrxz" \
  --executor "docker" \
  --docker-image alpine:latest \
  --description "docker-runner"
```

注册成功后的效果：

![](https://img.zhangniandong.com/2024/175.178.167.11_admin_runners_(1).jpg)
