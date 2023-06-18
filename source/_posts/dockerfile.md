---
title: dockerfile文件构建
cover: >-
  https://github.com/jjnian/images/blob/main/blog/%E7%81%AB%E8%BD%A6%E7%AB%99.png?raw=true
date: 2023-06-17 19:20:18
---

## docker

### docker原理

### docker命令

### [Dockerfile文件](https://docs.docker.com/engine/reference/builder/)

Dockerfile也可以写成dockerfile，一般写成Dockerfile，用于构建docker镜像

### Dockerfile文件构建

 docker文件中的命令大小写都可以，一般大写

 docker文件的一条命令占一行

- FROM

```dockerfile
# 构建镜像的基础，from必须在其他命令前面
# 可以写多条该命令
FROM xxx
```

- RUN

```dockerfile
# 构建镜像运行的命令
# 可以写多条
# 有多种写的格式，例如执行python app.py这个命令，有一下几种方式
RUN ["python", "app.py"]
RUN echo "Hello, Docker!"
RUN python app.py
```

- CMD

```dockerfile
# 在启动命令的时候执行
# 只能有一条，如果有多条，只执行最后一条
# 有多种写的格式，例如执行python app.py这个命令，有一下几种方式
CMD ["python", "app.py"]
CMD echo "Hello, Docker!"
CMD python app.py

# 只有 docker run 容器名称 这个种方式才能出发CMD，如果启动名称后面有其他参数就会覆盖掉CMD的默认命令
```



- COPY

```dockerfile
# 从宿主机复制文件到docker镜像中
# 可以写多条
# 宿主机路径可以写绝对路径，也可以写相对路径，相对路径以dockerfile文件所在路径为基础
COPY 宿主机文件路径 镜像文件路径
```

- ADD

```dockerfile
ADD 宿主机文件路径 镜像文件路径
ADD 文件的url地址 镜像文件路径
```

- ADD和COPY的区别

  - `COPY` 命令只能复制主机上的本地文件或目录到镜像中。
  - `ADD` 命令不仅可以复制主机上的本地文件或目录，还可以复制远程文件或 URL 到镜像中。如果目标路径是一个 URL，Docker 将尝试自动下载并复制文件到镜像中。

  

  - `COPY` 命令在复制过程中不会自动解压缩文件
  - `ADD` 命令在复制过程中如果遇到压缩文件（例如 `.tar`, `.tar.gz`, `.tgz`, `.zip`），它会自动解压缩文件并将其复制到镜像中

- ENV

```dockerfile
# 设置环境变量，可以通过$变量 获取值
# 可以设置多条
ENV 变量=赋值
```

- EXPOSE

```dockerfile
# 展示镜像的端口号信息
# EXPOSR 80 443 3306
# 在创建容器时需要根据-p参数和宿主机绑定
# 并不会真正打开容器的端口，只是提示信息，可以让使用者看到容器的端口情况
EXPOSR 端口 端口 端口
```

- WORKDIR

```dockerfile
# 设置镜像的工作目录
WORKDIR 目录
```

- VOLUME

```dockerfile
# 展示镜像可挂载点信息
# VOLUME /home/data1 /home/data2s
# 在运行容器时需要和宿主机通过-v参数绑定
VOLUME 路径 路径 路径
```

### .Dockerignore文件

该文件用于建立镜像文件时忽略文件，默认和dockerfile文件在同一目录下。gitignore相似，但在语法上有不同的地方。

### dockerignore和gitignore的区别

- dockerignore不会去递归匹配，需要**/
- dockerignore匹配文件夹需要后面带 /



应用场景：当我们在copy前端打包的文件到镜像中时，可以设置忽略部分文件不用复制

```dockerignore
main
    --node_modules
	  --public
	  --src
	  --xxx
	  --xxx
	  --xxx
	  --xxx
	  --xxx
Dockerfile
.Dockerignore
```

例如我们有以上层级的文件目录，我们需要把main文件夹下除node_modules文件夹的其他文件复制到镜像中，我们可以使用dockerignore文件配置忽略node_modules文件夹。

```dockerignore
# 配置这一行即可
**/node_modules/
```

## [docker-compose](https://docs.docker.com/compose/compose-file/)

### [docker-compose命令](https://docs.docker.com/compose/reference/)

- build镜像

```shell
# 打包docker-compose的镜像
docker-compose build
```



- 启动docker-compose

```shell
# -d是后台启动
docker-compose up -d
```

- 停止并删除docker-compose容器

```shell
docker-compose down
```

- 停止docker-compose容器

```shell
docker-compose stop
```

- 启动docker-compose容器

```shell
# 前提是要创建出容器
docker-compose start
```

### [docker-compose文件构建](https://docs.docker.com/engine/reference/commandline/cli/)

docker-compose需要建立docker-compose.yml或者docker-compose.yaml

- docker-compose.yml

docker-compose.yml文件命令忽略大小写，但是用大写的就老报错

docker-compose.yml文件有五大顶级元素

- version

```yml
# 这是docker-compose的版本号
version: "3.2"
```

- services

```yml
services:
  tomcat:
    # 容器名称
    container_name: tomcat01
    # 容器使用的镜像
    image: tomcat:8.0.53
    # 映射的端口
    ports:
      - "8080:8080"
    # 数据卷映射   宿主机地址:容器地址
    volumes:
      - /home/nginx/html:/usr/share/nginx/html
    # 该容器依赖的其他容器
    depends_on:
      - tomcat02
  tomcat02:
    # 容器名称
    container_name: tomcat02
    # 容器使用的镜像
    image: tomcat:8.0.53
   
```

- networks

```yml

```

- volumes

```yml

```

- configs

```yml

```

- secrets

```yml

```

