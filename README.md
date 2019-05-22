- [1. 架构概述](https://mp.csdn.net/mdeditor/90407643#1__1)
- [2. 技术解读](https://mp.csdn.net/mdeditor/90407643#2__26)
  - [2.1 node和npm](https://mp.csdn.net/mdeditor/90407643#21_nodenpm_27)
  - [2.2 Vue](https://mp.csdn.net/mdeditor/90407643#22_Vue_53)
  - [2.3 SPA](https://mp.csdn.net/mdeditor/90407643#23_SPA_66)
  - [2.4 Webpack](https://mp.csdn.net/mdeditor/90407643#24_Webpack_68)
  - [2.5 vue-cli](https://mp.csdn.net/mdeditor/90407643#25_vuecli_76)
  - [2.4 Vuetify](https://mp.csdn.net/mdeditor/90407643#24_Vuetify_89)

## 项目启动：live-server --port=9002

界面：

![img](https://img-blog.csdnimg.cn/20190520152807473.png?) 

# 1. 架构概述

前端有一套完整的技术栈，将来会去做独立的部署。

架构图如下：![在这里插入图片描述](https://img-blog.csdnimg.cn/20190521153200324.png?)

1.基于node，在node的基础上有npm和webpack，主要用于项目构建管理。
  - npm：项目的依赖管理
  - webpack：项目打包和编译
      &ensp;  两者合起来是maven的功能，但又比maven强大

2.在此基础上，Vue.js作为前端的主框架，基于Vue，又有两种：
  - vuetify：页面渲染，是一个UI框架，做页面样式。vue.js只负责渲染，没有样式
  - NUXT：服务端渲染(前端的服务端)，即用node搭建服务

前端与后端交互全部都通过ajax请求

***前端分为两部分***
  - 后台管理系统 
&ensp;  &ensp; 面向网站内部人员，因此采用Vue.js框架搭建出单应用SPA去做（方便）
  - 前端购物系统
&ensp;  &ensp; 面向用户这一套，采用结合Vue用NUXT服务端渲染（不能采用单应用，原因：单应用请求性能较差，首次加载的速度较慢，不方便做缓存与页面静态化；而NUXT服务端渲染可以做页面静态化，页面的访问效率较高，利于SEO的优化）

	不管哪种，将来都是通过ajax与后台进行交互。统一访问同一个微服务集群	
	们的后台管理系统 部署在nginx上，nginx是一个web服务器，部署前端代码，前端的静态资源都放在nginx上，与后端产生交互，后端的入口（即微服务集群的入口）是zuul，到了zuul，ribbon再做负载均衡与hystix	失败容错	去访问微服务群，微服务群会注册到eureka上去，同时，微服务之间要进行通信使用rabbitmq做一个异步通信，同步调用采用feign实现

# 2. 技术解读
## 2.1 node和npm
在node以前，前端只是用来写界面的，从node开始，它把引擎和与浏览器分开了，以前要想解析js只能在浏览器，而node.js将引擎抽取出来打造了一套框架，它可以让js运行在任何平台上。
注：node在项目中没有用到，只需了解一下
而node作为前端服务器，涌现出了一大批的前端框架，Vue就是其中之一

npm是Node提供的模块管理工具，可以非常方便的下载安装很多前端框架

&ensp;  &ensp;  npm安装好nodejs就自带了npm，但是默认是国外镜像，推荐使用淘宝镜像。但是切换镜像是比较麻烦的。推荐一款切换镜像的工具：nrm

我们首先安装nrm，这里-g代表全局安装：

```java
npm install nrm -g
```
指定要使用的镜像源：

```java
nrm use taobao
```

以后就可以使用npm安装了

***注：***
- nodejs是js后端运行平台，可以把它看成java体系中对应的jdk，是三个里面最基础的。
- npm是nodejs的包管理工具，可以把它看成maven中包依赖管理那部分。
- webpack是前端工程化打包工具，可以把它看成maven中工程自动化构建那部分。
## 2.2 Vue
*<font size=1>之前，在页面上发起ajax请求去获取后台数据，拿到数据后通过Dom操作来控制页面上的标签，从而呈现出动态效果，然后当用户在界面上操作完成后，再通过Dom操作去获取页面上的数据，最后通过ajax请求将数据发送到后台，从而实现页面视图与模型数据之间的交互。
而jQuery让Dom操作变得简单，但是需要人工把model渲染到view上。*

MVVM模式：把DOM操作完全封装起来，开发人员不用再关心Model和View之间是如何互相影响的，因此jQuery也用不上。
Vue：一款MVVM模式的框架，用于构建用户界面的渐进式框架

安装：

```java
npm install vue --save  #（--save表示本地安装，只针对当前项目使用）
```

## 2.3 SPA
SPA: Single Page Application，单应用，整个后台管理系统只有一个html页面，剩余一切页面的内容都是通过Vue组件(js文件)来实现
## 2.4 Webpack
Webpack：一个前端资源的打包工具，它可以将js、image、css等资源当成一个模块进行打包。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190521154731102.png?)

为什么需要打包？

- 将许多碎小文件打包成一个整体，减少单页面内的衍生请求次数，提高网站效率。
- 将ES6的高级语法进行转换编译，以兼容老版本的浏览器。
- 将代码打包的同时进行混淆，提高代码的安全性。
## 2.5 vue-cli
vue-cli：快速搭建vue项目的脚手架（包括webpack）

安装命令：

```java
npm install -g vue-cli
```
用vue-cli命令，快速搭建一个webpack打包的项目：

```java
vue init webpack
```
## 2.4 Vuetify
Vue负责的是虽然会帮我们进行js数据渲染，但是样式是有我们自己来完成。为了便于操作，引入Vuetify
Vuetify：基于Vue的UI框架，可以利用预定义的页面组件快速构建页面
