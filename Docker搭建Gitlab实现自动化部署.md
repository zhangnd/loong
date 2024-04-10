# Docker搭建Gitlab实现自动化部署

**更新yum**

```bash
yum update
```

```bash
yum install yum-utils
```

**添加docker阿里云镜像源**

```bash
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

**通过yum安装docker**

```bash
yum install docker-ce
```

**查看docker版本**

```bash
docker -v
```

**启动docker**

```bash
systemctl start docker
# 开机启动
systemctl enable docker
```

```bash
docker pull gitlab/gitlab-ce
```

文档：<a href="https://docs.gitlab.com/ee/install/docker.html" target="_blank">https://docs.gitlab.com/ee/install/docker.html</a>

```bash
export GITLAB_HOME=/srv/gitlab
```

```bash
docker run --detach \
  --hostname 公网ip \
  --env GITLAB_OMNIBUS_CONFIG="external_url 'http://gitlab.example.com'" \
  --publish 443:443 --publish 80:80 --publish 22:22 \
  --name gitlab \
  --restart always \
  --volume $GITLAB_HOME/config:/etc/gitlab \
  --volume $GITLAB_HOME/logs:/var/log/gitlab \
  --volume $GITLAB_HOME/data:/var/opt/gitlab \
  --shm-size 256m \
  gitlab/gitlab-ee:<version>-ee.0
```

```bash
docker pull gitlab/gitlab-runner
```

文档：<a href="https://docs.gitlab.com/runner/install/docker.html" target="_blank">https://docs.gitlab.com/runner/install/docker.html</a>
