1. docker images  

   > 查看镜像

2. docker pull {image}

   > 拉取镜像

3. docker inspect {image}

   > 查看镜像详情

4. docker rmi {image}

   > 移除镜像

5. docker create {image}

   > 根据镜像创建容器

6. docker  start {image}

   > 启动容器

7. docker run {image}

   > create , start 集合
   >
   > 例如：
   >
   > docker run -d --hostname my-rabbit --name some-rabbit -p 4369:4369 -p 5671:5671 -p 5672:5672 -p 25672:25672 -p 15672:15672 rabbitmq:3-management

8. docker stop {container}

   > 暂停容器

9. docker rm {container}

   > 删除容器

10. docker exec

   > 执行指定命令。通常使用sh、bash命令来对容器进行控制，和 -it 命令一起是使用。
   >
   > 例如：
   >
   > 1. docker exec -it some-rabbit bash
   > 
   
11. docker ps

   > 查看运行状态的容器

   \- as

   > 查看所有状态的容器

12. docker start

   > 启动暂停的docker容器
