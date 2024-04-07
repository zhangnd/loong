# 常用Git命令

查看用户名

```
git config user.name
```

查看邮箱

```
git config user.email
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

列出全局配置

```
git config --global -l
```

创建分支

```
git branch testing
```

切换分支

```
git checkout testing
```

创建并切换分支

```
git checkout -b iss53
```

删除分支

```
git branch -d hotfix
```

强制删除分支

```
git branch -D hotfix
```

删除远程分支

```
git branch -d -r hotfix
```
