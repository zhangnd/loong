# 常用npm命令

查看npm配置

```
npm config ls
```

查看npm镜像配置

```
npm config get registry
```

将npm设置为淘宝镜像

```
npm config set registry https://registry.npmmirror.com
```

切换为默认镜像

```
npm config set registry https://registry.npmjs.org
```

强制清除缓存

```
npm cache clean --force
```
