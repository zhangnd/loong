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

### docker

https://docs.docker.com/engine/install/centos

```bash
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
docker -v
docker compose version
```

### java

https://www.oracle.com/cn/java/technologies/downloads

```bash
mkdir /usr/local/java
wget https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.tar.gz
tar -zxvf jdk-17_linux-x64_bin.tar.gz
mv jdk-17.0.11 /usr/local/java
```

配置环境变量

```bash
vi /etc/profile
```

```
export JAVA_HOME=/usr/local/java/jdk-17.0.11
export PATH=$JAVA_HOME/bin:$PATH
```

```bash
source /etc/profile
java -version
```

### maven

https://maven.apache.org/download.cgi

```bash
mkdir /usr/local/maven
wget https://dlcdn.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz
tar -zxvf apache-maven-3.9.6-bin.tar.gz
mv apache-maven-3.9.6 /usr/local/maven
```

配置环境变量

```bash
vi /etc/profile
```

```
export M2_HOME=/usr/local/maven/apache-maven-3.9.6
export PATH=$M2_HOME/bin:$PATH
```

```bash
source /etc/profile
mvn -v
```

###

```bash
systemctl start docker
# 开机启动
systemctl enable docker
```

### 构建镜像

```bash
# 前端
docker build -t tiger:latest .
# 后端
mvn clean package
```

### 运行容器

```bash
docker compose up -d
docker compose logs -f
```

## 进入容器

```bash
# 前端
docker exec -it tiger /bin/sh
# 后端
docker exec -it mysql env LANG=C.UTF-8 /bin/bash
```
