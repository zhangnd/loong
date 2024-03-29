# 常用Git命令

## 配置

列出当前配置

```
git config -l
```

列出全局配置

```
git config --global -l
```

列出系统配置

```
git config --system -l
```

配置用户名

```
git config --global user.name "John Doe"
```

配置邮箱

```
git config --global user.email "johndoe@example.com"
```

凭证存储

```
git config --global credential.helper store
```

## 分支管理

克隆指定分支

```
git clone -b <name> <repository>
```

创建分支

```
git branch <branchname>
```

切换分支

```
git checkout <branch>
```

删除分支

```
git branch (-d | -D) <branchname>
```

删除远程分支

```
git branch (-d | -D) -r <branchname>
```

本地分支跟踪远程分支

```
git branch (--set-upstream-to=origin | -u origin)/<branchname> <branchname>
```

## 打标签

## 撤销

版本回退
