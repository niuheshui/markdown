# Node.js 安装(zip)



## [Node.js 官网](https://nodejs.org/zh-cn)



## 解压

解压并新建两个目录

node-global: npm全局安装位置

node-cache: npm缓存路径

## 配置环境变量

%NODE_PATH%

%NODE_PATH%\node-global

## 配置npm全局安装路径

管理员打开cmd

```shell
npm config set cache "D:\Node.js\node-v10.16.3-win-x64\node-cache"
npm config set prefix "D:\Node.js\node-v10.16.3-win-x64\node-global"`
```

## npm设置淘宝仓库

```shell
npm config set registry=https://registry.npm.taobao.org
```

## 查看镜像设置

```shell
npm get registry
```



