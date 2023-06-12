---
title: pm2学习文档
tags: 学习
categories: 前端
typora-copy-images-to: upload
top: 111
---

### 安装

```javascript
$ npm install pm2@latest -g
# or
$ yarn global add pm2
```

<!--more-->

### 启动应用

```javascript
pm2 start app.js

pm2 start "npm run start"

```

### 启动命令的一些配置参数

```javascript
//pm2进程的名字
--name <app_name>

//监听文件变化时重启
--watch

//内存超过设置大小后重启
--max-memory-restart <200MB>

//指定日志路径
--log <log_path>

//添加额外的参数
-- arg1 arg2 arg3

//多久重新启动一次
--restart-delay <delay in ms>

//日志带上时间
--time

//不自动重启
--no-autorestart

//指定cron强制重启
--cron <cron_pattern>

//附加到应用程序日志
--no-daemon

```

### pm2进程管理流程

```javascript
pm2 restart app_name
pm2 reload app_name
pm2 stop app_name
pm2 delete app_name
//app_name也可以换成对应的pm2进程id,当填写的时候all将作用于所有pm2进程

```

### 展示所有的pm2进程

```javascript
pm2 list
```

### 显示日志

```javascript
//实时显示日志
pm2 logs

//显示之前的日志
pm2 logs --lines 200

//显示特定进程的日志
pm2 logs app_name
pm2 logs id
```

### 显示终端的实时仪表板

```javascript
pm2 monit
```

### 生成ecosystem.config.js模版文件

```javascript

pm2 ecosystem

```

### ecosystem.config.js模版文件

```javascript
module.exports = {
  apps : [{
    name: "app",
    script: "./app.js",
    watch:true,//当目录有文件变化就重启
    watch: ["server", "client"],//当server和client文件有变化就重启
    watch_delay: 1000,监听之间的间隔
    ignore_watch : ["node_modules", "client/img"],
    //配合watch为true，忽略node_modules和client/img的文件的变化
    max_memory_restart: '300M',//当内存大于300M时就重启
    restart_delay: 3000,//设置重启间隔
    autorestart: false，//默认自动重启，设置为false关闭自动重启
    env: {
      NODE_ENV: "development",
    },
    env_production: {
      NODE_ENV: "production",
    }
  }, {
     name: 'worker',
     script: 'worker.js'
  }]
}
```

### ecosystem.config.js实例，使用于大部分node项目

```javascript
module.exports = {
  apps: [
    {
      name: "进程名字",
      script: "npm",
      args: "run start",
      exec_mode: "fork",
      instances: 1,
      autorestart: true,
      watch: false,
    },
  ],
};

```

### 同时运行多个应用程序

```javascript
module.exports = {
  apps : [{
      name: "进程名字",
      script: "npm",
      args: "run start",
  },{
         name: "进程名字",
      script: "npm",
      args: "run start",
  }]
}
```

### pm2脚本运行

```javascript
pm2 start ecosystem.config.js
```
