## NPM的安装目录

npm的模块都在node_modules下

可以通过一下命令查看

```
// 查看全局安装路径
npm root -g

// 查看npm的基础设置
npm config ls

// 查看安装目录路径
npm config get prefix
```

默认模块位置

/usr/locla/lib/node_modules



### QA

1. Q:为什么会有这么多node_modules目录

   A:https://www.cnblogs.com/weiyinfu/p/8471407.html

2. Q:FATAL Cannot read property 'code' of undefined是什么原因

   A:因为当前目录下存在package.json且不是有效配置文件  