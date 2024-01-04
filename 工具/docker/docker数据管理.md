## docker数据管理

​		docker的数据卷管理，能够让不同的容器共享数据。

1. **Bind Mount**

   > 指定宿主机目录作为容器目录
   >
   > doker run --mount

2. **Volume**

   > 由docker分配数据存储空间
   >
   > docker run -v {宿主机目录}:{容器目录}
   >
   > 由docker来分配容器目录在宿主机中的位置
   >
   > docker run -v {容器目录}

3. **Tmpfs Mount**

   > 临时数据挂载目录，存放在内存中。
   >
   > 适用于对于存储要求不要，但是需要频繁快速读取的数据
   >
   > --tmpfs

   通过volume命令能够创建数据卷

   > 创建数据卷
   >
   > docker volume create {volume}
   >
   > 删除数据卷
   >
   > docker volume rm {volume}
   >
   > 删除无用数据卷
   >
   > docker volume prune