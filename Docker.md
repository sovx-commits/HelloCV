**概念**

1. Docker与虚拟机的区别：每个虚拟机都自带一个完整的客户机操作系统，而所有Docker容器共享同一个主机操作系统的内核。
2. 镜像：创建容器的模板。
3. 容器：虚拟化操作系统内核，与主机共享操作系统，更加轻量化。
4. 仓库：集中存储和分发 Docker 镜像 的地方，使构建好的镜像能够被重复运行。
5. 分层镜像结构：Docker镜像的每个层都是只读的，每条指令都会创建一个新的层，它们共用一个基础层，提高存储效率。容器运行时，Docker会在所有只读层上运行一个可写层（容器层）。构建镜像时，若该指令及其下层与上一次构建相同，则直接调用缓存的层，不重复执行，使其快速构建和部署。拉取镜像时，只下载缺失层，达到快速拉取和推送的目的。
6. 卷：一个可供容器访问的存储空间，用于持久化数据。
7. 端口映射：绑定宿主机的一个端口和容器的一个端口。
8. 容器内部网络与宿主机之间的关系：容器与宿主机之间存在网络隔离，容器有自己的虚拟网络设备。
+ **Docker工作流程**：

         构建镜像--推送镜像--拉取镜像--运行容器





**容器管理**

+ **拉取、构建镜像**：

         $ docker pull NAME   #载入本地没有的镜像

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760889021802-a517c9dd-a3fc-4e13-a701-edf2cbb0d92d.png)

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760889031352-bb770a31-63c3-425f-9980-49a7631e6a6f.png)

+ **运行、管理容器**

**启动容器**

$ docker run -it ubuntu /bin/bash  #用Ubuntu镜像启动一个容器

-i: 交互式操作

-t: 终端

要退出终端，直接输入 exit:

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760936415657-0a0abf13-e39d-4623-b40d-c4d25678aa01.png)

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760936456857-e1bd2163-8467-4556-8c05-1b489482b8a4.png)

**启动已停止的容器**

1.查看所有容器

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760936585266-8526b47d-668c-4a5f-8049-63dcc10e7a28.png)

2.用docker start 启动已停止的容器![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760936699382-08de3ddf-b4d1-4c46-98f9-7b0cf2f53540.png)

         $ sudo docker run [...] 

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760889300221-12c7dc59-073a-4922-8887-0d68b11a9062.png)

**后台运行（-d）**

$ docker run -itd --name ubuntu-test ubuntu /bin/bash

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760936951943-85471960-44b1-4025-9b9c-cc993daf156c.png)

-d 默认不会进入容器，此时可使用docker exec 进入（用这个指令在推出容器终端时，容器不会停止。

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760937249179-310020b0-0116-4ee6-b54c-c458e30578c1.png)

**停止容器**

1. $ docker stop <容器 ID> #停止命令

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760937394838-fe815023-eb61-4f90-8df3-9dff4792843d.png)

2. $ docker restart <容器 ID> #重启停止的容器

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760937484198-6d92be3d-332b-4ec0-893b-b86cf329b780.png)

**导出和导入容器**

1.导出容器

$ docker export <容器ID>  > <本地文件>

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760938072502-227ceaa7-7253-438b-841f-0ceeccd44259.png)

2.导入容器

$ sudo import <快照文件> <镜像>

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760938422415-a474e8bd-ac2f-4cc3-91ff-4123338727ed.png)

**删除容器**

$ docker rm -f <容器ID>

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760938613606-c9bcc696-f027-4144-aaeb-a80c3c889e65.png)

**删除镜像**

$ docker rmi 镜像名：标签   /  镜像ID    #删除**镜像**（若删除多个则相互之间用空格隔开）

$ docker rmi -f 镜像ID #强制删除镜像（比如镜像在被某个容器使用时，无法直接删除的情况）

$ docker rmi -f $(docker images -q)   #删除所有镜像（括号中的命令是为了列出所有镜像）

**查看容器**

$ sudo docker ps -a #查看所有容器（包括已停止的）

$ sudo docker ps #查看运行中的容器

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760889505021-8e0424df-66a8-411d-8b76-bc402cad6375.png)

**容器日志**

docker logs <容器名或容器ID>  #查看容器最新日志

docker logs -f  <容器名或容器ID>  #实时跟踪日志(Ctrl + C 退出）

doxker logs --tail <容器名或容器ID>  #查看末尾N行

docker logs -t <容器名或容器ID> #显示时间

docker logs --since <过去时长/具体时间点> <容器名或容器ID>  #显示某个时间点后的日志

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760951090024-f0066dfb-5536-475f-9aea-eec721debd8b.png)

+ **基本的Dockerfile编写，自行构建镜像并运行服务**
1. 在一个空目录下，新建一个名为Dockerfile的文件

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760951828735-094bfe29-8259-4650-a416-7ebe8fd0ccb4.png)

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760951831152-3a66da5d-3ed4-4335-a0cb-55a8fa48aceb.png)

2. FROM和RUN指令

FROM：定制的镜像都基于FROM，这里的nginx就是定制需要的基础镜像，后续操作则基于nginx

RUN：1.shell格式：RUN <命令行命令>    （类似于终端输入 e.g. RUN apt        update）

             2.exec格式： ["可执行文件", "参数1", "参数2"]（e.g.  RUN ["apt-get",     "update"]

{注意不要创建过多无意义的层，可以用&&连接命令}

3. 开始构建镜像

$ docker build -t nginx:v3 .  （最后这个点是上下文路径，用于把本机指定目录下的文件提供给Docker引擎）

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760954258013-fa4f561e-c1ed-4fb6-aa4c-b13431d8e403.png)

出现网络问题，则尝试直接拉取镜像再构建：

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760954285887-5088a59f-8144-4168-aae1-e743364fe5d6.png)

4. 指令

**COPY**：从上下文目录中复制文件或者目录到容器里指定路径

COPY [--chown=<user>:<group>] <源路径1>...  <目标路径>    或者

COPY [--chown=<user>:<group>] ["<源路径1>",...  "<目标路径>"]  #适用于包含空格的路径

**CMD**：与RUN区分，CMD在docker run的时候运行（Dockerfile中仅最后一个         CMD指令生效）； RUN在docker build的时候运行

CMD <shell 命令> 

CMD ["<可执行文件或命令>","<param1>","<param2>",...] （推荐）

CMD ["<param1>","<param2>",...]  # 该写法是为 ENTRYPOINT 指令指定的程序提供默认参数

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760958596398-c0d2fdb3-0a1c-45fc-b70e-513535c84fe1.png)

**WORKDIR**:用 WORKDIR 指定的工作目录，会在构建镜像的每一层中都存在

WORKDIR <工作目录路径>

e.g.   WORKDIR /app

+ **容器与宿主机的数据和服务交互Docker Hub**

数据交互

docker volume create my_db_data

docker run -v my_db_data:/var/lib/postgresql/data postgres

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760960334516-8bbe574c-f408-4671-aa6b-08fa9060c6d5.png)

服务交互--端口映射

docker run -p 8080:80 nginx

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760960513130-3ecb645c-1dab-4f77-acde-188c5e67661a.png)



