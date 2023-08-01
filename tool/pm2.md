## pm2

> PM2 是进程管理工具，可以简化应用管理工作

#### 1. 常用命令

```git
pm2 ecosystem // 生成pm2配置文件，可直接作为start参数启动
pm2 list // 查看当前运行的pm2进程名
pm2 log // 显示实时日志
pm2 start // 启动进程
    --name：启动进程时将进程命名为
    --env：启动时执行环境
    --watch：监听应用目录的变化，一旦发生变化，自动重启。
    -i --instances：启用多少个实例，可用于负载均衡。如果-i 0或者-i max，则根据当前机器核数确定实例数目。
    --ignore-watch：排除监听的目录/文件，可以是特定的文件名，也可以是正则。
    -n --name：应用的名称，查看应用信息时可见。
    -o --output <path>：标准输出日志文件的路径。
    -e --error <path>：错误输出日志文件的路径。
    --interpreter <interpreter>：指定解释执行器来达到执行非Node.js项目的目的。
pm2 stop app_name/app_id/all // 终止进程
pm2 delete app_name/app_id/all // 删除进程
pm2 show // 查看指定应用详情
pm2 describe // 查看指定应用详情
```

#### 2. 配置文件格式

```git
module.exports = {
  apps : [{
    name: "app",
    script: "./app.js",
    env: {
      NODE_ENV: "development",
    },
    env_production: { // 启动时--env production指定
      NODE_ENV: "production",
    },
    "watch": [  // 监控变化的目录，一旦变化，自动重启
        "bin",
        "routers"
    ],
    "ignore_watch" : [  // 从监控目录中排除
        "node_modules",
        "logs",
        "public"
    ],
    "watch_options": {
        "followSymlinks": false
    },
    "error_file" : "./logs/app-err.log",  // 错误日志路径
    "out_file"   : "./logs/app-out.log",  // 普通日志路径
  }, {
     name: 'worker',
     script: './worker.js'
  }]
}
```
