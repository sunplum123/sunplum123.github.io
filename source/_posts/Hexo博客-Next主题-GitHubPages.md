---
title: Hexo博客-Next主题-GitHub Pages
date: 2017-01-01 10:46:00
categories: 休闲技术
tags:
 -blog
---

### 首先安装hexo
```javascript
// 全局安装 hexo 
$ npm install hexo -g 
```

### 初始化一个博客文件夹（hexo）
```javascript 
$ hexo init hexo

$ cd hexo 

$ npm install

$ hexo generate
// 本地简历
$ hexo server

```
>此时theme下有一个默认主题   

<!-- more -->
### 在hexo目录下，安装Next主题
```javascript
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```


### 创建一个github项目管理博客
#### 1、创建一个项目( sunplum123.github.io)
#### 2、创建两个分支：master和hexo
#### 3、设置hexo为默认分支
#### 4、clone此项目到本地
#### 5、进入项目的目录
```java
// 初始化hexo生成hexo站点，这个命令使得.git文件夹（此文件夹是隐藏状态）消失，应该先做好备份
$ hexo init
// 安装hexo 站点依赖
$ npm install
// 安装hexo git 部署插件
$ npm install hexo-deployer-git
```
#### 6、修改站点配置文件（_config.yml）中deploy属性，分支修改成master

#### 7. 依次执行git add .、git commit -m "..."、git push origin hexo提交网站相关的文件；
#### 8. 执行hexo g -d生成网站并部署到GitHub上。
