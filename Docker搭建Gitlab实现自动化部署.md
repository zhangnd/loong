# Docker搭建Gitlab实现自动化部署

CI/CD is a continuous method of software development, where you continuously build, test, deploy, and monitor iterative code changes.

CI/CD是一种连续的软件开发方法，你可以在其中不断构建、测试、部署和监视迭代代码更改。

## GitLab

### 硬件要求

文档：https://docs.gitlab.com/ee/install/requirements.html

CPU：Up to 20 Requests per Second (RPS) or 1000 users - 8 vCPU.

Memory：Up to 20 Requests per Second (RPS) or 1000 users - 8 GB (Minimum), 16 GB (Recommended).

上4核8G。

传送门：[腾讯云按量计费](https://buy.cloud.tencent.com/cvm?tab=custom&devPayMode=hourly)

公网ip：111.230.29.92

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
[root@VM-0-12-centos ~]# docker images
REPOSITORY         TAG       IMAGE ID       CREATED      SIZE
gitlab/gitlab-ce   latest    82056e2f7336   5 days ago   2.86GB
```

### 运行容器

文档：https://docs.gitlab.com/ee/install/docker.html

```bash
mkdir -p /srv/gitlab
```

```bash
docker run --detach \
  --hostname 111.230.29.92 \
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

浏览器访问：http://111.230.29.92

![](https://img.zhangniandong.com/2024/20240307175044.jpg)

初始用户名：root

初始密码：

```
[root@VM-0-12-centos ~]# cat /srv/gitlab/config/initial_root_password
# WARNING: This value is valid only in the following conditions
#          1. If provided manually (either via `GITLAB_ROOT_PASSWORD` environment variable or via `gitlab_rails['initial_root_password']` setting in `gitlab.rb`, it was provided before database was seeded for the first time (usually, the first reconfigure run).
#          2. Password hasn't been changed manually, either via UI or via command line.
#
#          If the password shown here doesn't work, you must reset the admin password following https://docs.gitlab.com/ee/security/reset_user_password.html#reset-your-root-password.

Password: 39Hedg/hjp98Hj85iaVrKLrng0qRt/DGioRmkcjst6o=

# NOTE: This file will be automatically deleted in the first reconfigure run after 24 hours.
```

登录成功。

![](https://img.zhangniandong.com/2024/20240307175232.jpg)

### 创建runner

文档：https://docs.gitlab.com/ee/ci/runners/runners_scope.html

打开：http://111.230.29.92/admin/runners

点击New instance runner。

![](https://img.zhangniandong.com/2024/20240307175424.jpg)

填写tag，点击Create runner。

![](https://img.zhangniandong.com/2024/20240307175458.jpg)

创建成功，得到runner authentication token：glrt-7mWJmg5B2wns7oN4QAv8

## GitLab Runner

Install GitLab Runner on a server separate to where GitLab is installed.

将GitLab Runner与GitLab分开部署。

在另一台服务器上部署GitLab Runner，配置采用2核4G。

公网ip：43.138.185.168

Docker部分同上。

### 安装runner

文档：https://docs.gitlab.com/runner/install/linux-repository.html

```bash
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh" | sudo bash
```

```bash
yum install gitlab-runner
```

### 注册runner

文档：https://docs.gitlab.com/runner/register/index.html

```bash
gitlab-runner register \
  --non-interactive \
  --url "http://111.230.29.92" \
  --token "glrt-7mWJmg5B2wns7oN4QAv8" \
  --executor "docker" \
  --docker-image alpine:latest \
  --description "docker-runner"
```

注册成功后的效果：

![](https://img.zhangniandong.com/2024/20240307181103.jpg)

### 构建项目

在项目根目录新建DockerFile文件。

```
FROM node:18.20.2-alpine as builder
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
RUN npm run build
FROM nginx:alpine
COPY --from=builder /app/dist/build/h5 /usr/share/nginx/html
COPY --from=builder /app/nginx.conf /etc/nginx/conf.d/default.conf
```

在项目根目录新建nginx.conf文件。

```
server {
  listen       80;
  server_name  localhost;

  location / {
    root       /usr/share/nginx/html;
    index      index.html index.htm;
    try_files  $uri $uri/ /index.html;
  }

  error_page   500 502 503 504  /50x.html;
  location = /50x.html {
    root   html;
  }
}
```

在项目根目录新建.gitlab-ci.yml文件。

```
stages:
  - deploy

deploy:
  stage: deploy
  tags:
    - shared
  only:
    - master
  image: docker
  script:
    - docker build -t $CI_PROJECT_NAME:$CI_PIPELINE_ID .
    - if [ $(docker ps -aq --filter name=$CI_PROJECT_NAME) ]; then docker rm -f $CI_PROJECT_NAME; fi
    - docker run -d -p 80:80 --name $CI_PROJECT_NAME $CI_PROJECT_NAME:$CI_PIPELINE_ID
```

预定义变量：https://docs.gitlab.com/ee/ci/variables/predefined_variables.html

提交代码到仓库，触发流水线构建。

作业如果报错：ERROR: error during connect: Head "http[]()://docker:2375/_ping": dial tcp: lookup docker on 183.60.83.19:53: no such host

可以修改/etc/gitlab-runner/config.toml：

```
volumes = ["/var/run/docker.sock:/var/run/docker.sock", "/cache"]
```

重启gitlab-runner。

```bash
gitlab-runner stop
gitlab-runner start
```

重新构建流水线成功。

浏览器访问：http://43.138.185.168
