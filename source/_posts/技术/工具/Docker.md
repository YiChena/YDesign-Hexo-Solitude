---
title: Docker
date: 2022-10-01 15:10:56
cover: https://ydesign.oss-cn-beijing.aliyuncs.com/DocKer.png
tags:
---

# **Docker**

----

### **Docker的概述**

---

<img src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fboke.yigujin.cn%2Fimg%2F2019%2F190128-docker.png&refer=http%3A%2F%2Fboke.yigujin.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1629890267&t=a1a511b3e4313239f168cd8baa6217ff" alt="点击查看源网页" style="zoom:150%;" />

#### **什么是Docker**

docker是一个用Go语言实现的开源项目，可以让我们方便的创建和使用容器，**docker将程序以及程序所有的依赖都打包到docker container（容器），这样你的程序可以在任何环境都会有一致的表现**

**docker可以屏蔽环境差异，只要你的程序打包到了docker中，那么无论运行在什么环境下程序的行为都是一致的**，**不会再有“在我的环境上可以运行”**，真正实现“一旦创建，到处运行”

- Docker 属于 Linux 容器的一种封装，提供简单易用的容器使用接口。它是目前最流行的 **Linux 容器解决方案**。
- Docker 将应用程序与该程序的依赖，打包在一个文件里面。运行这个文件，就会生成一个虚拟容器。程序在这个虚拟容
  器里运行，就好像在真实的物理机上运行一样。有了 Docker，就不用担心环境问题。
- Docker 的接口相当简单，用户可以方便地创建和使用容器，把自己的应用放入容器。容器还可以进行版
  本管理、复制、分享、修改，就像管理普通的代码一样。

#### **Docker的用途**

docker中有这样几个概念：

- **dockerfile**       构建进行的文本文件，对镜像进行操作
- **image**              docker镜像，相当于安装包
- **container **       docker 容器，相当于运行的程序
- **仓库（Repository）**：类似于代码仓库，这里是镜像仓库，是Docker用来集中存放镜像文件的地方

实际上你可以简单的把image(镜像)理解为**可执行程序（程序安装包）**，container(容器)就是运行起来的进程(运行起来的任务)。

那么写程序需要源代码，那么“写”image就需要dockerfile，dockerfile就是image的源代码，docker就是"编译器"docker进行“编译”。



**Docker 的主要用途**，目前有三大类。
（1）提供一次性的环境。比如，本地测试他人的软件、持续集成的时候提供单元测试和构建的环境。
（2）提供弹性的云服务。因为 Docker 容器可以随开随关，很适合动态扩容和缩容。
（3）组建微服务架构。通过多个容器，一台机器可以跑多个服务，因此在本机就可以模拟出微服务架构。

#### **Docker与Linux的区别**



### **Docker基本组成**

<img src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fi.loli.net%2F2020%2F08%2F01%2F9VSxDTPkYJ5n6eZ.png&refer=http%3A%2F%2Fi.loli.net&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1629803170&t=e2dd4969ba04481a529432712ffcb5c9" alt="img" style="zoom:150%;" />

#### **docker执行原理**

**通过客户端输入相应的命令，控制服务器端运行的过程**

实际上docker使用了常见的CS架构，也就是client(客户端)-server(服务器)模式，docker client(客户端)负责处理用户输入的各种命令，比如docker build、docker run，真正工作的其实是server(服务器)，也就是docker demon（docker守护进程），值得注意的是，docker client和docker demon可以运行在同一台机器上

#### **docker工作流程**

**通过docker最基础的核心命令，操作守护进程，进而控制整个docker的运行**

##### **docker build**

**编译dockerfile** 

当我们写完dockerfile交给docker“编译”时使用**docker build**这个命令，那么client在接收到请求后转发给docker daemon，接着docker daemon根据dockerfile创建出“可执行程序”的image(镜像)

<img src="C:\Users\lhw\AppData\Roaming\Typora\typora-user-images\image-20210726194313700.png" alt="image-20210726194313700" style="zoom:150%;" />

##### **docker run**

**将镜像加载进内存，将镜像执行成容器**

有了“可执行程序”image后就可以运行程序了，接下来使用命令**docker run**，docker daemon接收到该命令后找到具体的image，然后**加载到内存开始执行**，image执行起来就是所谓的container(容器)

<img src="C:\Users\lhw\AppData\Roaming\Typora\typora-user-images\image-20210726194131560.png" alt="image-20210726194131560" style="zoom:150%;" />

docker build和docker run是两个最核心的命令，会用这两个命令基本上docker就可以用起来了

##### **docker pull**

**向仓库请求下载镜像文件**

用户通过docker client(客户端)发送命令，docker daemon(守护线程)接收到命令后向docker registry(仓库)发送image下载请求，下载后存放在本地，这样我们就可以使用image

<img src="C:\Users\lhw\AppData\Roaming\Typora\typora-user-images\image-20210726233005020.png" alt="image-20210726233005020" style="zoom: 200%;" />

这里的仓库有公开仓库和本地仓库

- 公开仓库：docker Hub
- 本地仓库：本地下载好的镜像仓库



**docker的底层实现**

docker基于Linux内核提供这样几项功能实现的：

- **NameSpace**
  我们知道Linux中的PID、IPC、网络等资源是全局的，而NameSpace机制是一种资源隔离方案，在该机制下这些资源就不再是全局的了，而是属于某个特定的NameSpace，各个NameSpace下的资源互不干扰，这就使得每个NameSpace看上去就像一个独立的操作系统一样，但是只有NameSpace是不够。
- **Control groups**
  虽然有了NameSpace技术可以实现资源隔离，但进程还是可以不受控的访问系统资源，比如CPU、内存、磁盘、网络等，为了控制容器中进程对资源的访问，Docker采用control groups技术(也就是cgroup)，有了cgroup就可以控制容器中进程对系统资源的消耗了，比如你可以限制某个容器使用内存的上限、可以在哪些CPU上运行等等。

有了这两项技术，容器看起来就真的像是独立的操作系统了。



----

### **Docker的安装**

---



#### **Win安装Docker**

省略

#### **Mac安装Docker**

省略

#### **Linux安装Docker**

**Linux安装Docker以CentOS为例**

##### **切除旧环境**

旧版本的docker环境或者版本。如果安装了这些，则卸载它们以及相关的依赖关系。`docker`和`docker-engine`

```linux
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

##### **安装Docker**

在新主机上首次安装 Docker Engine 之前，您需要设置 Docker 存储库。之后，您可以从存储库安装和更新 Docker。

###### **设置存储库（设置仓库）**

安装`yum-utils`包（提供`yum-config-manager` 实用程序）。

```shell
sudo yum install -y yum-utils
```

**设置仓储**

设置**稳定**存储库

```shell
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```



###### **安装 Docker 引擎**

安装*最新版本*的 Docker Engine 和 containerd

```shell
sudo yum install -y  docker-ce docker-ce-cli containerd.io
```

Docker 已安装但未启动。该`docker`组被创建，但没有用户添加到组

###### **启动 Docker**

无论何时何地，重新登录或重新使用docker时，都要启动Docker

```shell
 sudo systemctl start docker
```

###### **测试Docker**

通过运行`hello-world` 映像验证 Docker Engine 是否已正确安装

```shell
sudo docker run hello-world
```

Docker 运行程序是统一使用：

```shell
docker run 运行的程序名
```

**有sudo 和 无sudo区别**

- Docker 引擎已安装并正在运行。您需要使用`sudo`来运行 Docker 命令(启用超级用户权限)。
- 不使用sudo ：继续[Linux postinstall](https://docs.docker.com/engine/install/linux-postinstall/)以允许非特权用户运行 Docker 命令和其他可选配置步骤（非root用户，管理Docker）。

###### **非root用户管理Docker**

**其他用户操作Docker进程只能使用sudo**

Docker 守护进程绑定到 Unix 套接字而不是 TCP 端口。默认情况下，Unix 套接字归用户所有`root`，其他用户只能使用`sudo`. Docker 守护进程始终以`root`用户身份运行。

**不使用sudo，直接创建组名docker的用户**

如果您不想在`docker`命令前加上`sudo`，请创建一个名为的 Unix 组`docker`并向其中添加用户。当 Docker 守护进程启动时，它会创建一个可由`docker`组成员访问的 Unix 套接字。

**要创建`docker`组并添加您的用户**：

1. 创建`docker`组。

   ```shell
   $ sudo groupadd docker
   ```

2. 将您的用户添加到`docker`组中。

   ```shell
   $ sudo usermod -aG docker $USER
   ```

3.注销并重新登录，以便重新评估您的组成员身份,测试docker组用户是否添加成功

```shell
 newgrp docker 
```



**配置Docker默认开机启动**

大多数当前的 Linux 发行版（RHEL、CentOS、Fedora、Debian、Ubuntu 16.04 及更高版本）用于[`systemd`](https://docs.docker.com/engine/install/linux-postinstall/#systemd)管理系统启动时启动的服务（非默认开机启动）。在 Debian 和 Ubuntu 上，Docker 服务默认配置为在启动时启动（开机自启动）。

要**设置开机启动**，需要在引导时为其他发行版自动启动 Docker 和 Containerd，请使用以下命令

```shell
 sudo systemctl enable docker.service
 sudo systemctl enable containerd.service
```

**禁用开机启动**

要禁用开机行为，请`disable`改用

```shell
sudo systemctl disable docker.service
sudo systemctl disable containerd.service
```



##### **删除Docker**

1. **卸载 Docker Engine、CLI 和 Containerd 包**：

   ```shell
   $ sudo yum remove docker-ce docker-ce-cli containerd.io
   ```

2. **主机上的映像、容器、卷或自定义配置文件不会自动删除。删除所有镜像、容器和卷**：

   ```shell
   $ sudo rm -rf /var/lib/docker
   $ sudo rm -rf /var/lib/containerd
   ```

您必须手动删除任何已编辑的配置文件。

##### **镜像加速**

###### **阿里云镜像加速**

docker默认镜像在国外，国内使用很慢这里直接换成阿里云镜像

**操作**

![image-20200616154429105](D:\桌面\狂神说笔记\crazy-god-says-java-master\37、docker\Docker.assets\image-20200616154429105.png)

![image-20200616154455964](D:\桌面\狂神说笔记\crazy-god-says-java-master\37、docker\Docker.assets\image-20200616154455964.png)

控制台搜索 容器镜像服务

![image-20200616155201285](D:\桌面\狂神说笔记\crazy-god-says-java-master\37、docker\Docker.assets\image-20200616155201285.png)

找到加速地址

![image-20200616155649476](D:\桌面\狂神说笔记\crazy-god-says-java-master\37、docker\Docker.assets\image-20200616155649476.png)

```
sudo mkdir -p /etc/docker # 创建一个陌路
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://uyfgafsw.mirror.aliyuncs.com"]
}
EOF # 编写配置文件

sudo systemctl daemon-reload # 重启服务
sudo systemctl restart docker # 重启docker
```

![image-20200616160315298](D:\桌面\狂神说笔记\crazy-god-says-java-master\37、docker\Docker.assets\image-20200616160315298.png)



---

### **Docker常用命令**

#### **帮助命令**

```shell
docker version        #显示命令信息
docker info           #显示docker系统信息  包括镜像和容器的数量
docker 命令  --help   #帮助命令，查看该命令的使用

```

帮助文档:查看所有命令：https://docs.docker.com/reference

---

#### **镜像命令**

**镜像命令(images)**

**所有进行命令都可以查询Docker官网 查看**

镜像命令帮助文档:https://docs.docker.com/engine/reference/commandline/images/

##### **查询镜像**

**docker images 查看所有本地的主机镜像**

```shell
[root@localhost docker]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    d1165f221234   4 months ago   13.3kB

#解释
REPOSITORY       镜像的仓库源
TAG       		镜像的标签
IMAGE ID        镜像的ID
CREATED         镜像的创建时间
SIZE		   镜像的大小
#常用查看镜像命令(可选项)
[root@localhost docker]# docker images --help

  -a,    --all        #列出所有镜像

  -q, --quiet         #只显示镜像的Id

[root@localhost docker]# docker images -a    #列出所有镜像
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    d1165f221234   4 months ago   13.3kB
[root@localhost docker]# docker images -q    #只显示镜像的Id
d1165f221234

```

##### **搜索镜像**

**docker search 搜索镜像**

例如搜索mysql

```shell
[root@localhost ~]# docker search mysql
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   11168     [OK]       
mariadb                           MariaDB Server is a high performing open sou…   4238      [OK]       
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   830                  [OK]
percona                           Percona Server is a fork of the MySQL relati…   548       [OK]       
phpmyadmin                        phpMyAdmin - A web interface for MySQL and M…   279       [OK]     

#可选项

```

##### **下载镜像**

**Docker pull 下载镜像**

如下载mysql

```shell
#下载镜像 Docker pull 镜像名[:tag ]  可选版本
[root@localhost ~]# docker pull mysql
Using default tag: latest   #如果不写tag 默认就是 latest 最后就新版本
latest: Pulling from library/mysql
33847f680f63: Pull complete   #分层下载  docker image的核心  相同冲突部分不需要下载，对其他不同部分更新就行
5cb67864e624: Pull complete 
1a2b594783f5: Pull complete 
b30e406dd925: Pull complete 
48901e306e4c: Pull complete 
603d2b7147fd: Pull complete 
802aa684c1c4: Pull complete 
715d3c143a06: Pull complete 
6978e1b7a511: Pull complete 
f0d78b0ac1be: Pull complete 
35a94d251ed1: Pull complete 
36f75719b1a9: Pull complete 
Digest: sha256:8b928a5117cf5c2238c7a09cd28c2e801ac98f91c3f8203a8938ae51f14700fd
Status: Downloaded newer image for mysql:latest   
docker.io/library/mysql:latest   #真实地址

docker pull mysql 等价于 docker.io/library/mysql:latest   #真实地址

#指定版本下载
[root@localhost ~]# docker pull mysql:5.7
5.7: Pulling from library/mysql
33847f680f63: Already exists 
5cb67864e624: Already exists 
1a2b594783f5: Already exists 
b30e406dd925: Already exists 
48901e306e4c: Already exists 
603d2b7147fd: Already exists 
802aa684c1c4: Already exists 
5b5a19178915: Pull complete 
f9ce7411c6e4: Pull complete 
f51f6977d9b2: Pull complete 
aeb6b16ce012: Pull complete 
Digest: sha256:be70d18aedc37927293e7947c8de41ae6490ecd4c79df1db40d1b5b5af7d9596
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7

```

查看下载后的版本

<img src="C:\Users\lhw\AppData\Roaming\Typora\typora-user-images\image-20210725134236320.png" alt="image-20210725134236320" style="zoom:150%;" />

##### **删除镜像**

**docker rmi 删除镜像**

```shell
[root@localhost ~]# docker rmi -f 镜像ID                  #删除指定的镜像镜像镜像
[root@localhost ~]# docker rmi -f 镜像ID 镜像ID 镜像ID     #删除多个的镜像镜像
[root@localhost ~]# docker rmi -f $(docker images -aq)    #删除所有镜像

```

#### **使用镜像**

##### **构建镜像的两种方法**

- 使用docker commit将容器打包成一个镜像
- 编写Dockerfile文件之后使用docker build编译命令构造新的镜像

现在我们并不推荐使用Docker commit 命令 构建新的镜像， 而是使用灵活的更强大的Dockerfine来构建Docker镜像，

因为使用Docker commit 命令 构建新的镜像。

- 麻烦，不安全，
- 要新建一个容器，将容器修改，然后才是提交成新的镜像



##### **docker commit**

**docker commit将容器打包成一个镜像**

docker commit 主要是将一个容器创建成为一个镜像

使用场景：有一个容器已经搭建好了环境，部署好了代码和其他软件，可以正常运行测试环境了，为了以后可以直接使用这个容器，就需**要用docker commit将这个容器创建成为镜像（容器打包成镜像环境，方便其他地方直接使用），以后直接使用这个镜像就好了**

```shell
#新建一个容器
$ sudo docker -i -t centos /bin/bash
#对容器进行个性化定制
root@12345685865: yum install git 
root@12345685865: yum install jdk-11.012
#退出容器
exit
#使用commit命令对容器保存：容器转换为镜像
#docker commit的使用
docker commit [option] 容器 [镜像:版本]
-a :提交的镜像作者；
-c :使用Dockerfile指令来创建镜像；
-m :提交时的说明文字；
-p :在commit时，将容器暂停。

#下面是将容器my_mysql创建为镜像，镜像名称是mymysql
#使用commit 命令将容器打包成镜像
$ docker commit -a "jack" -m "my_mysql" 21he8dt67q93 mymysql:v1
#查看镜像
$ docker images mymysql:v1
```



##### **Dockerfile**

构建一个DockerFile实例

构建步骤：

1. 编写一个dockerfile文件
2. docker build 构建成为一个镜像
3. docker run运行镜像
4. docker push 发布镜像（DockerHub、阿里云镜像仓库 私有/共有）

**Dockerfile相当于对镜像操作的脚本**

镜像的定制实际上就是定制每一层所 **添加的配置、文件**。如果我们可以把每一层修改、安装、构建、操作的命令都写入一个脚 本，用这个脚本来构建、定制镜像，那么之前提及的无法重复的问题、镜像构建透明性的问题、体积的问题就都会解决。这个脚本就是 Dockerfile。

###### **编写Dockerfile**

```shell
#使用Dockerfile文件
#新建名字MyDockerfile文件来保存Dockefile脚本命令
touch MyDockerfile

#Dockerfile文件的语法
FROM   #指定基础镜像
MAINTAINER #指定构造该镜像的作者是谁
RUN  #用来执行命令行命令的
     #如果有多层文件，则使用&& 来关联串起来。
     
#例如
#Version：0.01   					   #定义构建镜像的版本
FROM  centos 7.1    					#现在构建的基础镜像
MAINTAINER  LHW    						#指定作者的个人信息
RUN  yum update && yum install -y git     # 启动容器，先是更新yum 源 ，再下载 git 
EXPOSE 80  							    #指定端口号，由于端口号为80 则任意自由访问该容器
```

被FROM 选择的是基础镜像，然后经过build命令编译Dockerfile的是新镜像，新镜像以基础镜像为基础镜像构建。

**编译Dockerfile文件构建新的镜像**

**通过build命令，编译Dockerfile文件构建新的镜像**

```shell
#执行Dockerfile文件格式
docker build [选项] <上下文路径/URL/->

#例如:
#执行build命令的Dockerfile文件编译过程
$ docker build -t nginx:v3 .   #执行Dockerfile 文件 其中.为当前目录
Sending build context to Docker daemon 2.048 kB
Step 1 : FROM nginx       #第一步选择基本镜像
---> e43d811ce2f4      
Step 2 : RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html    #启动容器，执行操作
---> Running in 9cdc27646c7b      #运行在id为9cdc27646c7b 的容器
---> 44aa4490ce2c            	  #生成为新的id为44aa4490ce2c的镜像
Removing intermediate container 9cdc27646c7b       #打印运行信息        
Successfully built 44aa4490ce2c
```

###### **Dockerfile常用命令**

```shell
FROM # 基础镜像 比如centos
MAINTAINER # 镜像是谁写的 姓名+邮箱
RUN # 镜像构建时需要运行的命令
ADD # 添加，比如添加一个tomcat压缩包
WORKDIR # 镜像的工作目录
VOLUME # 挂载的目录
EXPOSE # 指定暴露端口，跟-p一个道理
RUN # 最终要运行的
CMD # 指定这个容器启动的时候要运行的命令，只有最后一个会生效，而且可被替代
ENTRYPOINT # 指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILD # 当构建一个被继承Dockerfile 这个时候运行ONBUILD指定，触发指令
COPY # 将文件拷贝到镜像中
ENV # 构建的时候设置环境变量
```







---

#### **容器命令**

**镜像是源码实体，容器只是镜像的实例化**

通过在Docker 下载一个centos  演示我们有了镜像才可以创建容器

**在docker 安装一个centos**

```shell
docker pull centos
```

##### **新建容器并启动**

通过镜像生成相应容器

/bin/bash是一个用户终端窗口

```shell
docker run [可选参数] 镜像名

#可选参数说明
-- name="Name"      容器名字  类型于 tomcat01 tomcat02  区分容器
-d                  后台运行
-it                 使用交互方式运行，进入容器查看内容
-p                  指定容器端口 -p 8080:8080
		-p ip:主机端口:容器端口
		-p 主机端口:容器端口
		-p 容器端口
		容器端口
-P                  随机指定端口


#测试，并启动进入容器
[root@localhost ~]# docker run -it centos /bin/bash（/bin/bash是一个用户终端窗口）
[root@0998f1e0393d /]# ls     #查看容器内的centos
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@0998f1e0393d /]# exit   #退出centos容器服务器

```

##### **查看正在运行的容器**

**查看正在运行的docker容器**

```shell
#docker ps 命令  #查看正在运行的容器
		#列出正在运行的容器
-a		#列出当前正在运行的容器，和带出历史运行过的容器
-n=?	#只显示最近创建的容器
-q		#只显示容器编号
		
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@localhost ~]#  docker ps -a -n=1   #只显示最近运行的一个容器
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS                       PORTS     NAMES
0998f1e0393d   centos    "/bin/bash"   9 minutes ago   Exited (127) 5 minutes ago             sweet_hodgkin
[root@localhost ~]# docker ps -aq   #只显示当前容器id
0998f1e0393d
56a0ada84984
40d7defc4c65
7e90c0876013
4953d0050112

```

##### **退出容器**

```shell
exit     	  #直接容器停止并退出
Ctrl + P + Q  #容器不停止，但退出
```

##### **删除容器**

**大白话理解：镜像是源码实体，容器只是镜像的实例化，就可以理解容器就相当于app一样**

```shell
docker rm 容器id               #删除指定的容器，不能删除正在运行的容器，如果要强制删除 rm-f
docker rm -f $(docker ps -aq)  #删除所有容器
```

##### **启动和停止容器**

```shell
docker start 容器id        #启动容器
docker restart 容器id      #重新启动容器
docker stop 容器id         #停止当前正在运行的容器
docker kill 容器id         #强制停止当前容器

[root@localhost ~]# docker  start 0998f1e0393d     #启动容器
0998f1e0393d
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
0998f1e0393d   centos    "/bin/bash"   26 minutes ago   Up 13 seconds             sweet_hodgkin
[root@localhost ~]# docker stop 0998f1e0393d     #停止当前正在运行的容器
0998f1e0393d
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

#### **其他常用的容器命令**

##### **后台启动容器**

```shell
#命令docker run -d 镜像名
[root@localhost ~]# docker run -d centos
930af1e70498120e780df7690796c4f9ea88f07731c74350acbda590d7c5f0c8
#问题docker ps 发现centos 停止了
#常见的坑： docker容器使用后台运行，就必要有一个前台程序，没有前台程序，Docker就没发现应用，就会停止
#nginx: 容器启动后，发现自己没有提供服务，就会停止，就是没有程序了
```

##### **查看日志**

```shell
#显示日志
--tf     	     #显示日志
[root@localhost ~]# docker logs -tf  56a0ada84984(容器id)   #显示容器所有日志
--tail number      #显示日志的条数
[root@localhost ~]# docker logs -tf --tail 10 56a0ada84984(容器id)
```

##### **查看容器中的进程信息**

```shell
#命令docker top 容器id
[root@localhost ~]# docker top 56a0ada84984
UID         PID        PPID         C         STIME        TTY         TIME        			CMD
root       4803       4783          0         18:02        pts/0       00:00:00            /bin/bash
```

##### **查看镜像的元数据**

```shell
#命令
docker inspect 容器id
#测试
[root@localhost ~]# docker inspect 56a0ada84984
[
    {	#容器id
        "Id": "56a0ada849842959766493861b3577789b6f88b29e678bf369fc916aacd27ff2",
        #创建时间
        "Created": "2021-07-25T06:19:05.373391492Z",
        #环境变量
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 4803,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-07-25T10:03:00.491873756Z",
            "FinishedAt": "2021-07-25T17:13:14.807892799+08:00"
        },
        "Image": "sha256:300e315adb2f96afe5f0b2780b87f28ae95231fe3bdd1e16b9ba606307728f55",
        "ResolvConfPath": "/var/lib/docker/containers/56a0ada849842959766493861b3577789b6f88b29e678bf369fc916aacd27ff2/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/56a0ada849842959766493861b3577789b6f88b29e678bf369fc916aacd27ff2/hostname",
        "HostsPath": "/var/lib/docker/containers/56a0ada849842959766493861b3577789b6f88b29e678bf369fc916aacd27ff2/hosts",
        "LogPath": "/var/lib/docker/containers/56a0ada849842959766493861b3577789b6f88b29e678bf369fc916aacd27ff2/56a0ada849842959766493861b3577789b6f88b29e678bf369fc916aacd27ff2-json.log",
        "Name": "/fervent_hodgkin",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/f1f9695b5608c4e662872681dc7b6fb1830e247d4d8aa738c4be9990ec117e24-init/diff:/var/lib/docker/overlay2/93f4a2fa8f812fae70e920ea1075850e8862c32066563f6c0fc4b83b80e002b2/diff",
                "MergedDir": "/var/lib/docker/overlay2/f1f9695b5608c4e662872681dc7b6fb1830e247d4d8aa738c4be9990ec117e24/merged",
                "UpperDir": "/var/lib/docker/overlay2/f1f9695b5608c4e662872681dc7b6fb1830e247d4d8aa738c4be9990ec117e24/diff",
                "WorkDir": "/var/lib/docker/overlay2/f1f9695b5608c4e662872681dc7b6fb1830e247d4d8aa738c4be9990ec117e24/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "56a0ada84984",
            "Domainname": "",
            "User": "",
            "AttachStdin": true,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": true,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20201204",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "12a6837241c9aafde43566e0c974716c2dea1f03bda1715e2cf2c797c95c33f8",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/12a6837241c9",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "d753b5d4b60df5e2b7783a3fbd4f42bfc4e65c801587abe7984eb8486584093f",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "74dfac2a61936d5413449b0966ff462092b3c342ace2992f09356e029c546df0",
                    "EndpointID": "d753b5d4b60df5e2b7783a3fbd4f42bfc4e65c801587abe7984eb8486584093f",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

##### **进入容器命令**

```shell
#我们通常容器都是使用后台运行的方式运行，需要进入容器，修改一些配置

#命令
docker exec -it 容器id /bin/bash

#测试
[root@localhost ~]# docker exec -it 56a0ada84984 /bin/bash
[root@56a0ada84984 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@56a0ada84984 /]# ps -ef
UID         PID   PPID  C STIME TTY          TIME CMD
root          1      0  0 10:02 pts/0    00:00:00 /bin/bash
root         15      0  0 10:22 pts/1    00:00:00 /bin/bash

#方法二
docker  attach 容器id
#测试
[root@localhost ~]# docker attach 56a0ada84984

#docker exec    #进入一个容器后开启一个新的终端，可以在里面操作
#docker attach   #进入一个容器正在执行终端，不会开始新进程
```

##### **从容器内拷贝文件到主机上**

```shell
docker cp 容器id:容器路径   目的的主机路径
#进入容器
[root@localhost home]# docker attach 56a0ada84984
[root@56a0ada84984 /]# cd /home/
#在容器新建一个文件
[root@56a0ada84984 home]# touch test.java
[root@56a0ada84984 home]# ls
test.java
#退出容器
[root@56a0ada84984 home]# exit
exit
[root@localhost home]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
#查看历史进程
[root@localhost home]# docker ps -a
CONTAINER ID   IMAGE         COMMAND       CREATED          STATUS                         PORTS     NAMES
603efea3fa92   centos        "/bin/bash"   42 minutes ago   Exited (0) 42 minutes ago                festive_wiles
502c3a688032   centos        "/bin/bash"   43 minutes ago   Exited (0) 42 minutes ago                nervous_bardeen
930af1e70498   centos        "/bin/bash"   55 minutes ago   Exited (0) 55 minutes ago                sharp_fermi
0998f1e0393d   centos        "/bin/bash"   2 hours ago      Exited (0) About an hour ago             sweet_hodgkin
56a0ada84984   centos        "/bin/bash"   4 hours ago      Exited (0) 25 seconds ago                fervent_hodgkin
40d7defc4c65   hello-world   "/hello"      6 hours ago      Exited (0) 6 hours ago                   gracious_jepsen
7e90c0876013   hello-world   "/hello"      26 hours ago     Exited (0) 26 hours ago                  frosty_goldwasser
4953d0050112   hello-world   "/hello"      29 hours ago     Exited (0) 29 hours ago                  eager_colden
#将文件拷贝出来到主机上
[root@localhost home]# docker cp 56a0ada84984:/home/test.java /home
#查看从容器中的文件是否拷贝成功
[root@localhost home]# ll
总用量 0
drwx------. 2 lhw    lhw    62 4月  27 10:37 lhw
drwx------. 2 lhw123 lhw123 90 4月  30 23:39 lhw123
-rw-r--r--. 1 root   root    0 7月  25 18:43 test.java


#拷贝只是手动过程，未来使用 -v卷的技术，可以实现自动同步拷贝
```

##### **镜像与容器小结**

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2F5b0988e595225.cdn.sohucs.com%2Fimages%2F20190925%2Fcc896018961147da9e1fb4cd4f49e429.jpeg&refer=http%3A%2F%2F5b0988e595225.cdn.sohucs.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1629803073&t=3c156f631a88b3b708a2ec903a59ad5e)

**小结：最常用镜像和容器命令**

```shell
attach    Attach to a running container  #当前shell下attach连接指定运行镜像
build     Build an image from a Dockerfile  #通过Dockerfile定制镜像
commit    Create a new image from a containers changes  #提交当前容器为新的镜像
cp    Copy files/folders from a container to a HOSTDIR or to STDOUT  #从容器中拷贝指定文件或者目录到宿主机中
create    Create a new container  #创建一个新的容器，同run 但不启动容器
diff    Inspect changes on a containers filesystem  #查看docker容器变化
events    Get real time events from the server#从docker服务获取容器实时事件
exec    Run a command in a running container#在已存在的容器上运行命令
export    Export a containers filesystem as a tar archive  #导出容器的内容流作为一个tar归档文件(对应import)
history    Show the history of an image  #展示一个镜像形成历史
images    List images  #列出系统当前镜像
import    Import the contents from a tarball to create a filesystem image  #从tar包中的内容创建一个新的文件系统映像(对应export)
info    Display system-wide information  #显示系统相关信息
inspect    Return low-level information on a container or image  #查看容器详细信息
kill    Kill a running container  #kill指定docker容器
load    Load an image from a tar archive or STDIN  #从一个tar包中加载一个镜像(对应save)
login    Register or log in to a Docker registry#注册或者登陆一个docker源服务器
logout    Log out from a Docker registry  #从当前Docker registry退出
logs    Fetch the logs of a container  #输出当前容器日志信息
pause    Pause all processes within a container#暂停容器
port    List port mappings or a specific mapping for the CONTAINER  #查看映射端口对应的容器内部源端口
ps    List containers  #列出容器列表
pull    Pull an image or a repository from a registry  #从docker镜像源服务器拉取指定镜像或者库镜像
push    Push an image or a repository to a registry  #推送指定镜像或者库镜像至docker源服务器
rename    Rename a container  #重命名容器
restart    Restart a running container  #重启运行的容器
rm    Remove one or more containers  #移除一个或者多个容器
rmi    Remove one or more images  #移除一个或多个镜像(无容器使用该镜像才可以删除，否则需要删除相关容器才可以继续或者-f强制删除)
run    Run a command in a new container  #创建一个新的容器并运行一个命令
save    Save an image(s) to a tar archive#保存一个镜像为一个tar包(对应load)
search    Search the Docker Hub for images  #在docker
hub中搜索镜像
start    Start one or more stopped containers#启动容器
stats    Display a live stream of container(s) resource usage statistics  #统计容器使用资源
stop    Stop a running container  #停止容器
tag         Tag an image into a repository  #给源中镜像打标签
top       Display the running processes of a container #查看容器中运行的进程信息
unpause    Unpause all processes within a container  #取消暂停容器
version    Show the Docker version information#查看容器版本号
wait         Block until a container stops, then print its exit code  #截取容器停止时的退出状态值
```

### **常见应用部署**

#### **安装nginx**

##### **搜索镜像**

建议去DockerHub搜索相应的版本和帮助文档

```shell
[root@localhost home]# docker search nginx
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
nginx                             Official build of Nginx.                        15203     [OK]       
jwilder/nginx-proxy               Automated Nginx reverse proxy for docker con…   2051                 [OK]
richarvey/nginx-php-fpm           Container running Nginx + PHP-FPM capable of…   816                  [OK]
jc21/nginx-proxy-manager          Docker container for managing Nginx proxy ho…   222                  
linuxserver/nginx                 An Nginx container, brought to you by LinuxS…   149                  
tiangolo/nginx-rtmp               Docker image with Nginx using the nginx-rtmp…   136                  [OK]
jlesage/nginx-proxy-manager       Docker container for Nginx Proxy Manager        125                  [OK]
alfg/nginx-rtmp                   NGINX, nginx-rtmp-module and FFmpeg from sou…   102                  [OK]
jasonrivers/nginx-rtmp            Docker images to host RTMP streams using NGI…   92                   [OK]
nginxdemos/hello                  NGINX webserver that serves a simple page co…   70                   [OK]
privatebin/nginx-fpm-alpine       PrivateBin running on an Nginx, php-fpm & Al…   56                   [OK]
nginx/nginx-ingress               NGINX and  NGINX Plus Ingress Controllers fo…   55                   
nginxinc/nginx-unprivileged       Unprivileged NGINX Dockerfiles                  45                   
staticfloat/nginx-certbot         Opinionated setup for automatic TLS certs lo…   24                   [OK]
nginx/nginx-prometheus-exporter   NGINX Prometheus Exporter for NGINX and NGIN…   19                   
schmunk42/nginx-redirect          A very simple container to redirect HTTP tra…   19                   [OK]
nginxproxy/nginx-proxy            Automated Nginx reverse proxy for docker con…   16                   
centos/nginx-112-centos7          Platform for running nginx 1.12 or building …   15                   
centos/nginx-18-centos7           Platform for running nginx 1.8 or building n…   13                   
bitwarden/nginx                   The Bitwarden nginx web server acting as a r…   11                   
flashspys/nginx-static            Super Lightweight Nginx Image                   10                   [OK]
mailu/nginx                       Mailu nginx frontend                            9                    [OK]
sophos/nginx-vts-exporter         Simple server that scrapes Nginx vts stats a…   7                    [OK]
ansibleplaybookbundle/nginx-apb   An APB to deploy NGINX                          2                    [OK]
wodby/nginx                       Generic nginx                                   1                    [OK]
```

##### **下载镜像**

下载镜像，使用pull命令

```shell
#下载nginx镜像
[root@localhost home]# docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
33847f680f63: Already exists 
dbb907d5159d: Pull complete 
8a268f30c42a: Pull complete 
b10cf527a02d: Pull complete 
c90b090c213b: Pull complete 
1f41b2f2bf94: Pull complete 
Digest: sha256:8f335768880da6baf72b70c701002b45f4932acae8d574dedfddaf967fc3ac90
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
#查看是否下载镜像成功
[root@localhost home]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
nginx         latest    08b152afcfae   3 days ago     133MB
```

##### **测试运行**

```shell
[root@localhost ~]# docker run -d --name nginx01 -p3344:80 nginx
6a67b3dfbe7d9f16a8f3035d42334f08eaecc5298242c2d0a0958aeee3fd86de
[root@localhost ~]# docker ps
CONTAINERID    IMAGE         COMMAND                CREATED         STATUS         PORTS                 NAMES
6a67b3dfbe7d   nginx     "/docker-entrypoint.…"   9 seconds ago   Up 6 seconds   0.0.0.0:3344->80/tcp, :::3344->80/tcp   ngnix01
```

**访问测试页面**

```shell
[root@localhost ~]# curl localhost:3344
```

**测试页面**

```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

<img src="C:\Users\lhw\AppData\Roaming\Typora\typora-user-images\image-20210725205304379.png" alt="image-20210725205304379" style="zoom:150%;" />

**端口暴露**

了解端口暴露问题：就是通过映射方式，外部访问端口与内部端口连接，在不影响内部端口的情况下，从而达到操作内部端口作用

<img src="C:\Users\lhw\AppData\Roaming\Typora\typora-user-images\image-20210725203739810.png" alt、 2="image-20210725203739810" style="zoom:150%;" />

**进入nginx容器**

**进入nginx改动配置文件**

```shell
#进入nginx容器
[root@localhost ~]# docker exec -it nginx01  /bin/bash
root@6a67b3dfbe7d:/# whereis nginx
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx
#查看nginx容器的配置文件
root@6a67b3dfbe7d:/# cd /etc/nginx
root@6a67b3dfbe7d:/etc/nginx# ls
conf.d	fastcgi_params	mime.types  modules  nginx.conf  scgi_params  uwsgi_params
```

##### **nginx的相关概念**

**正向代理代理的对象时客户端**

**反向代理代理的对象时服务端。**

代理：我们常说的代理就是正向代理，**它隐藏了真实请求的客户端，客户端请求的服务都被代理服务器代替来请求**，服务器端并不知道真实的客户端。

反向代理：**反向代理隐藏了真正的服务端**，当我们请求http://www.baidu.com时，百度可能有成千上万台服务器，**具体哪一台正在为我们服务，我们无需知道**，我们只需要知道反向代理服务器就可以了。



#### **安装tomact**

**认证进入容器命令**

##### **安装tomcat方法**

方法1：

**官方安装方法**

```shell
#官方使用
docker run -it --rm tomcat:9.0
#我们启动时都是后台，停止了容器之后，容器还是可以查到， docker run -it --rm 一般用来测试，用完就删除
```

方法二：

**最常用的tomcat安装方法**

```shell
#正确使用方法
#下载再启动
docker pull tomcat

#启动运行
docker run -d -p 3344:8080 --name tomcat01  tomcat

#测试没有问题
[root@localhost ~]# docker run -d -p 3344:8080 --name tomcat01 tomcat
2e5726cb194671b6c2f0f6b1c3ec8877e70c1ac1a24b890eec258bc94a0c2828
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND            CREATED          STATUS         PORTS              NAMES
2e5726cb1946   tomcat   "catalina.sh run"   10 seconds ago   Up 8 seconds   0.0.0.0:3344->8080/tcp, :::3344->8080/tcp   tomcat01
```

##### **进入容器**

```shell
#进入容器
[root@localhost ~]# docker exec -it tomcat01 /bin/bash
#发现问题
1.linux命令少了
2.没有webapp   #镜像默认最小化，所以很多不必要都删除了
#解决问题
没有webapp的情况：webapps.dist 默认有root 相关文件
root@2e5726cb1946:/usr/local/tomcat/webapps.dist# ls
ROOT  docs  examples  host-manager  manager
#我们将webapps.dist 的文件复制一份到webapp 即可
root@2e5726cb1946:/usr/local/tomcat# cp -r  webapps.dist/*  webapps
root@2e5726cb1946:/usr/local/tomcat# cd webapps
root@2e5726cb1946:/usr/local/tomcat/webapps# ls
ROOT  docs  examples  host-manager  manager
```



#### **部署ES+Kibanan**

##### **安装ES**

```shell
#ES的缺点
#es 暴露端口特别多
#es 十分耗费内存
#es的数据一般需要放置到安全位置目录，需要挂载

#启动elasticsearch
#discover.type=single-node  配置成单节点
docker run -d --name elasticsearch  -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2
```

##### **查看CPU 状态**

```shell
#启动就卡住了，使用docker stats 查看cpu 状态
docker stats 容器id
#测试一下es是否成功
curl localhost:9200
#es 是十分耗费内存的

```

##### **增加内存限制**

```shell
#赶紧关闭，内存限制，修改配置文件 -e 环境配置修改
docker run -d --name elasticsearch  -p 9200:9200 -p 9300:9300 -e  ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2
```



使用Kibanna连接es ? 思考网络如何才能连接过去！！！

![image-20210726174622292](C:\Users\lhw\AppData\Roaming\Typora\typora-user-images\image-20210726174622292.png)













