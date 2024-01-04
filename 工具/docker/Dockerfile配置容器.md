通过dockerfile能过更快的自定义镜像。通过 docker build 来构建自己的镜像。

```
FROM：基础镜像，当前新镜像是基于哪个镜像的

MAINTAINER：镜像维护者的姓名和邮箱地址

RUN：容器构建时需要运行的命令

EXPOSE：当前容器对外暴露出的端口

WORKDIR：指定在创建容器后，终端默认登陆的进来工作目录，一个落脚点

ENV：用来在构建镜像过程中设置环境变量

ADD：将宿主机目录下的文件拷贝进镜像且 ADD 命令会自动处理 URL 和解压 tar 压缩包

COPY：类似 ADD，拷贝文件和目录到镜像中。（COPY src dest 或 COPY ["src","dest"]）

VOLUME：容器数据卷，用于数据保存和持久化工作

CMD：指定一个容器启动时要运行的命令，Dockerfile 中可以有多个 CMD 指令，但只有最后一个生效，CMD 会被 docker run 之后的参数替换

ENTRYPOINT：指定一个容器启动时要运行的命令，ENTRYPOINT 的目的和 CMD 一样，都是在指定容器启动程序及参数

ONBUILD：当构建一个被继承的 Dockerfile 时运行命令，父镜像在被子继承后父镜像的 onbuild 被触发
```