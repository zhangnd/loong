# Docker搭建Gitlab实现自动化部署

### 4核8G服务器

好贵一个月，可以使用[腾讯云按量计费](https://buy.cloud.tencent.com/cvm?tab=custom&devPayMode=hourly)，用完关机。

### 更新yum

```bash
yum update
```

### 安装docker

```bash
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install docker-ce
docker -v
```

### 启动docker

```bash
systemctl start docker
# 开机启动
systemctl enable docker
```

### 拉取gitlab-ce镜像

ce为社区免费版。

```bash
docker pull gitlab/gitlab-ce
```

```
[root@VM-0-10-centos ~]# docker images
REPOSITORY         TAG       IMAGE ID       CREATED      SIZE
gitlab/gitlab-ce   latest    883ec00180cd   8 days ago   2.87GB
```

### 运行gitlab容器

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

### gitlab部署成功

浏览器访问：http://175.178.167.11

![](https://img.zhangniandong.com/2024/175.178.167.11_users_sign_in.png)

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

登录进去。

![](https://img.zhangniandong.com/2024/175.178.167.11_.png)

### 拉取gitlab-runner镜像

```bash
docker pull gitlab/gitlab-runner
```

```
[root@VM-0-10-centos ~]# docker images
REPOSITORY             TAG       IMAGE ID       CREATED       SIZE
gitlab/gitlab-ce       latest    883ec00180cd   8 days ago    2.87GB
gitlab/gitlab-runner   latest    1d176dab5774   12 days ago   766MB
```

### 运行gitlab-runner容器

文档：https://docs.gitlab.com/runner/install/docker.html

```bash
docker run -d --name gitlab-runner --restart always \
  -v /srv/gitlab-runner/config:/etc/gitlab-runner \
  -v /var/run/docker.sock:/var/run/docker.sock \
  gitlab/gitlab-runner:latest
```

### 安装gitlab-runner

文档：https://docs.gitlab.com/runner/install/linux-repository.html

```bash
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh" | sudo bash
yum install gitlab-runner
```

### 创建runner

文档：https://docs.gitlab.com/ee/ci/runners/runners_scope.html#create-an-instance-runner-with-a-runner-authentication-token

打开：http://175.178.167.11/admin/runners

点击New instance runner。

填写tag，点击Create runner。

创建成功，得到token：glrt-5AZqiUy5yypqwLuhGrxz

### 注册runner

文档：https://docs.gitlab.com/runner/register

```bash
sudo gitlab-runner register \
  --non-interactive \
  --url "http://175.178.167.11" \
  --token "glrt-5AZqiUy5yypqwLuhGrxz" \
  --executor "docker" \
  --docker-image alpine:latest \
  --description "docker-runner"
```

注册成功后的效果：

![](https://img.zhangniandong.com/2024/175.178.167.11_admin_runners.png)

### 构建项目

在项目根目录新建DockerFile文件。

```bash
FROM node:alpine as builder
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
RUN npm run build
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY --from=builder /app/nginx.conf /etc/nginx/conf.d/default.conf
```

在项目根目录新建nginx.conf文件。

在项目根目录新建.gitlab-ci.yml文件。

```bash
stages:
  - deploy

deploy:
  stage: deploy
  tags:
    - docker
  only:
    - dev
  image: docker
  script:
    - docker build -t $CI_PROJECT_NAME:$CI_PIPELINE_ID .
    - if [ $(docker ps -aq --filter name=$CI_PROJECT_NAME) ]; then docker rm -f $CI_PROJECT_NAME; fi
    - docker run -d -p 80:80 --name $CI_PROJECT_NAME $CI_PROJECT_NAME:$CI_PIPELINE_ID
```

预定义变量：https://docs.gitlab.com/ee/ci/variables/predefined_variables.html

提交代码到仓库，触发pipeline流水线构建：
