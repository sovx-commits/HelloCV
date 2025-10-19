**一、卸载旧版本的Docker**

$ sudo apt-get remove docker docker-engine docker.io containerd runc![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760760222823-b535357e-7aff-4d0a-b29c-cd163b4c57bd.png)



**二、使用Docker仓库进行安装**

1.**更新**

$ sudo apt-get update

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760760370163-4173fc3d-a850-4b17-9462-564199f751da.png)

2.**安装apt依赖包**

$ sudo apt install ca-certificates curl gnupg 

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760761348539-aadd9a92-70cb-417a-88c5-c0fbb0ebcf9d.png)

3.**创建Docker的GPG密钥目录**

$ sudo install -m 0755 -d /etc/apt/keyrings

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760761469796-625bb91d-413a-44e6-9659-bf21290ae82f.png)

4.**下载并添加GPG密钥**

$ curl -fsSL [https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg](https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg) | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

sudo chmod a+r /etc/apt/keyrings/docker.gpg

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760761587319-df0e775b-7fc6-4aac-b8d5-717fe2d5dc79.png)

5.**添加Docker仓库**

$ echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] [https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu](https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu) $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760761679932-c9c1c8ab-29d6-4fa4-ad99-787b2eff5974.png)

6.**更新并安装Docker**

$ sudo apt update

$ sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760762285823-765ae7cb-f77f-472e-9418-f3c2ca95f8cd.png)

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760762388696-a74efc48-85c1-476e-89e8-1f1bb095225a.png)

**三、测试Docker是否安装成功**

$ sudo docker run hello-world

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760762493967-f8e629a7-bb92-4544-a1c8-740af90e6dfc.png)

遇到**问题A：Docker守护进程未运行**

解决方案：

（1）**启动Docker服务**

$ sudo systemctl start docker

$sudo systemctl status docker

$sudo systemctl enable docker

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760762790831-dbcd478a-d01b-42d3-9347-915d76aa9328.png)

(2)**若上述方法无效，尝试重启Docker**

$ sudo systemctl restart docker

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760762946717-7a015fee-d5e0-44ca-aa84-86f5a25004a8.png)

这里我们遇到了**问题B：Docker服务启动失败**



解决方案：**排查关键步骤**

a.**查看Docker守护进程的直接输出**

    $ sudo dockerd --debug

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760763789821-d5f19b4b-5b21-4dae-9e50-fc63cffed756.png)

     从终端输出可以看出，问题源于/etc/docker/daemon.json文件的JSON语法错误，        出现了非法字符“h”。

     解决步骤：1.**备份并清空错误配置文件**（执行命令重命名当前的daemon.json）

                               $ sudo mv /etc/docker/daemon.json /etc/docker/daemon.json.bak

                           2.**重启Docker服务**

                               $ sudo systemctl restart docker

                           3.**验证服务状态**（检查Docker是否成功启动）

                               $ sudo systemctl status docker.service![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760764472738-9f1603fc-c4f9-49a6-b888-107b777ae392.png)

                               看到active (running)说明问题解决，若未成功，则执行以下步骤继续                                   排查。

b.**检查内核模块与依赖**

   确保Docker所需的内核模块已加载

   $ sudo modprobe overlay

   $ sudo modprobe br_netfilter

   验证内核网络转发配置

   $ cat /proc/sys/net/ipv4/ip_forward

   若输出为0,执行$ sudo sysctl -w net.ipv4.ip_forward=1 启用转发

c.**检查Docker配置文件**（同上a中问题）



再次测试

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760766113584-43405eda-411f-4322-af97-2a75e08dd542.png)

出现了**问题C：网络连接超时的错误**

**1.检查网络连接**

$ ping registry-1.docker.io #确认能否连通Docker官方镜像仓库

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760767022149-172fa90a-333c-4e45-8c72-1c7fc3e53de3.png)

我们可以看到，虽然可以Ping通，但是数据包100%丢失，这时候执行下一步。

**2.配置Docker镜像加速**

**（1）选择国内镜像加速器**

**          **以阿里云为例；

           1.浏览器访问阿里云容器镜像服务官网(https://cr.console.aliyun.com)

           2.左侧导航栏“镜像工具”下，进入“镜像加速器”页面

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760769450111-63e43a43-589e-444d-86f2-a4492b51a7ae.png)

           3.获取专属加速器地址 

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760769645636-b419c1e4-0adc-4f1c-ba6c-5960b2020df7.png)

**（2）配置Docker守护进程**

**           **$ sudo nano /etc/docker/daemon.json

           替换加速器地址,如下图：

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760770066754-8f5a7fd2-5021-461b-9554-c8358196f0cd.png)

**（3）重启Docker服务**

**           **$ sudo systemctl daemon-reload

           $ sudo systemctl restart docker

**（4）测试镜像拉取**

           $ sudo docker run hello-world

**问题D：Docker拉取镜像时仍连接到官方仓库**

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760771246280-e95b6ac7-1674-4a02-a575-c058dad4d687.png)

(a.确认加速器配置是否生效

![](https://cdn.nlark.com/yuque/0/2025/jpeg/61414303/1760771581206-f79b134d-81a9-4088-a81a-cf38ce29d09b.jpeg)

(b.重启Docker服务使配置生效

$ sudo systemctl daemon-reload

$ sudo systemctl restart docker

(c.验证加速器是否生效

$ docker info | grep "Registry Mirrors"

若输出中包含你配置的加速器地址，则配置成功

(d.再次最终检测

$ sudo docker run hello-world



**卸载Docker及其组件**

**1.停止Docker相关服务**

$ sudo systemctl stop docker docker.socket containerd

**2.卸载Docker核心程序包**

$ sudo apt-get purge -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760798359386-443597f6-1fa5-4898-8e38-051d155c6dc4.png)

**3.删除残留数据与配置目录**

$ sudo rm -rf /var/lib/docker

$ sudo rm -rf /var/lib/containerd

$ sudo rm -rf /etc/docker

**4.清理Docker相关二进制文件（彻底收尾）**

$ sudo rm -rf /usr/bin/docker* /usr/bin/containerd* /ussr/bin











****









