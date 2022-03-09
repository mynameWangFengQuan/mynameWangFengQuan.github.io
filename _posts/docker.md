> 结合文档：C:\杂项\开课吧资料\第十四章 容器化开发-docker\第二节 docker\docker.pdf

#### 1.2 安装docker

##### 镜像加速器

```
从网址https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors登陆后，左侧菜单选中 镜像加速器就可以看到你的专属地址了
需要登录自己的阿里云，我的加速器地址：https://myiv0n78.mirror.aliyuncs.com
```

#### 3.docker命令

> 操作镜像的命令，镜像产生容器的命令，整个daemon进程操作docker的命令

##### 3.2镜像相关的命令

镜像内容包括如下内容：查看镜像、搜索镜像、拉取镜像、删除镜像

镜像是从中央仓库中下载到本地host里面用来产生容器的，一个镜像可以产生多个容器

> 查看镜像：查看本地所有的镜像

```shell
docker images
docker images -q # 查看所用镜像的id
```

> 搜索镜像：从网络中查找需要的镜像

```
docker search 镜像名称
docker search redis # 查找redis镜像
```

拉取镜像：从Docker仓库下载镜像到本地，镜像名称格式为 名称：版本号，如果版本号不指定则是最新的版本lastest

> 如果不知道镜像版本，可以去docker  hub搜索对应镜像查看

> 删除镜像

```
docker rmi `docker images -q` 删除所有本地镜像  注意：这里不是单引号，是Tab上面的小飘号
```

##### 3.3容器相关的命令

查看容器、创建容器、进入容器、启动容器、停止容器、删除容器、查看容器信息 

> 查看容器 

```
docker ps # 查看正在运行的容器 
docker ps –a # 查看所有容器 
docker ps -aq 能够得到所有容器的编号列表
```

> 创建并启动容器 

```
docker run 参数 
```

参数说明： 

- -i：保持容器运行。通常与 -t 同时使用。加入it这两个参数后，容器创建后自动进入容器中，退出容器后，容 器自动关闭。 

- -t：为容器重新分配一个伪输入终端，通常与 -i 同时使用。 

- -d：以守护（后台）模式运行容器。创建一个容器在后台运行，需要使用docker exec 进入容器。退出后，容 器不会关闭。 

- -it 创建的容器一般称为交互式容器，-id 创建的容器一般称为守护式容器 

- --name：为创建的容器命名。 

  ```
  docker run -it --name=c1 centos:7 /bin/bash #创建交互式容器 
  	因为it会分配一个伪终端，所以需要初始化指令、/bin/bash
  docker run -id --name=c2 centos:7 #创建守护式容器 
  ```

![image-20210523101148298](C:\Users\WFQ\AppData\Roaming\Typora\typora-user-images\image-20210523101148298.png)

> 从docker到了容器里面，现在已经没法使用docker命令了，通过exit退出容器，但是it创建的容器退出后就会关闭了

![image-20210523101350685](C:\Users\WFQ\AppData\Roaming\Typora\typora-user-images\image-20210523101350685.png)

注意：交互式容器，exit后容器自动关闭，守护式容器会在后台执行 

> 进入容器 

```
docker exec -it c2 /bin/bash #进入容器停止容器 
```

> 停止容器

```
docker stop 容器名称
```

> 启动容器 

```
docker start 容器名称
```

> 删除容器：如果容器是运行状态则删除失败，需要停止容器才能删除 

```
docker rm 容器名称
docker rm `docker ps -aq` 注意，这里也是飘号
```

> 查看容器信息 

```
docker inspect 容器名称
```

#### 4.Docker容器的数据卷

##### 4.1数据卷概念及作用

思考：

- Docker容器删除后，容器中产生的数据还在吗？

  沙箱机制，不会存在

- Docker容器和外部机器可以直接交换文件吗？

  不可以，外部机器不可以直接访问容器，但是可以访问宿主机，宿主机可以访问容器

- 容器之间想要进行数据交互？

  这个就涉及到了数据卷

##### 4,3配置数据卷容器

容器挂载在数据卷容器上，其实数据卷容器关闭，容器仍会和数据卷进行挂载

#### 5.Docker应用部署

##### 5.1MySQL部署

###### 3.创建容器，设置端口映射、目录映射

```sh
# 在/root目录下创建mysql目录用于存储mysql数据信息 
mkdir ~/mysql 
cd ~/mysql
```

```sh
docker run -id \ 
-p 3307:3306 \ 
--name=c_mysql \ 
-v $PWD/conf:/etc/mysql/conf.d \  # 对mysql的配置、日志、数据进行挂载，以便外部机通过宿主机进行控制
-v $PWD/logs:/logs \  # $PWD 表示当前目录
-v $PWD/data:/var/lib/mysql \ 
-e MYSQL_ROOT_PASSWORD=123456 \ 
mysql:5.6

\表示换行的意思
一般在实际情况中，端口映射，宿主机和容器使用同一个端口
```

#### 6.2镜像制作

##### 6.2.1容器转镜像

![image-20210523175351366](C:\Users\WFQ\AppData\Roaming\Typora\typora-user-images\image-20210523175351366.png)

> 新镜像进行压缩时，挂载在数据卷中的内容不会进行保存转换
>
> 容器转镜像不能转换数据卷文件

```
docker build -f ./springboot_dockerfile -t app .
注意：文档中的build写错了
```

#### 7.服务编排

##### 7.3编排nginx+springboot

###### 2.编写docker-compose.yml文件

```yml
services: 
	nginx: 
		image: nginx 
		ports: 
			- 80:80 
		links: # 在当前容器连接其他容器，这样就可以用app这个别名访问其他容器
			- app 
		volumes: # 数据卷挂载
			- ./nginx/conf.d:/etc/nginx/conf.d 
		app:
			image: app 
			expose: 
				- "8080"
```

###### 3.创建./nginx/conf.d目录

###### 4.在./nginx/conf.d目录下编写app.conf文件

```c
访问80端口是自动代理到proxy_pass网址，可以使用app而不是ip进行访问，是因为上面的link配置
server { 
	listen 80; 
	access_log off; 
	location / { 
		proxy_pass http://app:8080/hello; 
	} 
}
```

> docker-compose应该在他的配置文件所在的目录中进行启动

