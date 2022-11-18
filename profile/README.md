# 客服系统

## 概述

采用`Golang`语言，基于 `gRPC` + [`Mongo`+ `Vue3`+`Websocket`+`RabbitMQ`]来实现系统

用户侧逻辑采用匿名注册模式，`Token`采用`JWT`技术，服务端不保存`Token`信息，用户每次使用检测`Token`合法时返回新的`Token`来达到续期的目的


后端分为 `客户服务`，`客服服务`，`客户用户中心`,`客服用户中心`


* `客户服务`: 创建、打开会话，发送、接受消息，关闭会话
* `客服服务`: 与客户前端通过`gRPC` 双向流来通信沟通。内容包括: 与当前服务的客户发送、接收消息，等待服务的客户列表、变动，开始、停止服务客户
* 客户用户中心: 检测、生成`Token`
* 客服用户中心: 注册、登录

前端当前仅实现了Web端，通过`websocket`服务器与后端沟通


## 后端

### `allinone` - `gRPC` [`Mongo`]

适用于单体服务器能支撑业务的场景，所有服务都集成在一个程序里, 所有客户、客服的沟通都通过回调函数来解决

> 配置文件中 `Dev->UseMemoryModel` 来配置是使用内存还是`Mongo`来保存用户和会话信息. 如果内存保存，则服务重启后数据清零

### 分离服务 - `gRPC` `Mongo` `RabbitMQ`

  * `客户服务`
  * `客服服务`
  * 客户用户中心
  * `客服用户中心`

> 服务可横向扩展

## 前端

### 原生`App` - 未实现

* `gRPC`

### Web浏览器

* 客户`Web`端
* 客服`Web`端

### Web浏览器与后端服务流中专服务

* 客户`WebSocket`服务器
* 客服`WebSocket`服务器


## 体验

### 本地编译运行

#### 前提

* 安装`Golang`, `npm`,`vue`

#### 下载运行

```bash
# 下载库
git clone https://github.com/zservicer/talkbe.git
git clone https://github.com/zservicer/talkfe.git

# 开一个窗口，运行后端程序
cd talkbe
cp config.yaml.allinone.sample config.yaml
cp ws_config.yaml.sample ws_config.yaml
go run cmd/allinone/main.go

# 开一个窗口,运行前端程序
cd talkfe
npm install
npm run server

```

#### 体验

客服端: 浏览器打开`http://127.0.0.1:8080`

> 具体端口号可以看`npm run server`的提示。
>
> 登录用户名/密码为: demo/123456

用户端: 浏览器打开`talkfe`工程的`demo/customer/talk.html`



### 在线Demo

客服端: https://kf.ymipro.com/

> 登录用户名/密码为: demo/123456

用户端: https://www.ymipro.com/

## 运行截图

### 客服端

![客服端](https://patdz.github.io/1999/11/17/htuchuang/servicer.png)

### 客户端

![客服端](https://patdz.github.io/1999/11/17/htuchuang/customer.png)

