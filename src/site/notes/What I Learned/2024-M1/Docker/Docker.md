---
{"dg-publish":true,"permalink":"/What I Learned/2024-M1/Docker/Docker/"}
---


> 在 KodeKloud 学习了一门 Docker 的入门课程，发现 GPT 写的 Docker 指令以及 DockerFile 完全正确。

![](https://s2.loli.net/2024/02/01/csW2aEglQwdIeF4.jpg)

Docker architecture:

![](https://pic.aspi-rin.top/2024/02/a95ee43a193774d0363e0307bd7c37b4.jpeg)

## Docker Commands

```
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

docker run nginx   # pull from docker hub, then start a container
docker pull nginx  # just download

docker run --name <Container name> <Image name>  # 指定 name
docker run -d <Image name>:<tag>  # detach 后台启动，指定版本
docker attach <Container ID/name>  # 回到 docker 内部

docker run -p 9000:80 xxx:1.0.0  # PORT mapping [外部:Docker内]
docker run –v /opt/datadir:/var/lib/mysql mysql  # Volume mapping

docker run -it nginx:latest /bin/bash  # 交互模式启动，在容器内执行 bash

docker exec -it <Container ID/name> /bin/bash/  # 进入容器的 Shell

docker stop <Container ID/name>  # 输入 id 的前几个字符即可
docker start <Container ID/name>  # 启动已有的 Container

docker rm <Container ID/name>  # 删除 Container
docker rmi <Image name> # 删除 Image
```

```
docker ps
docker ps -a

docker images  # list all the images

docker inspect <Container ID/name>  # JSON 格式输出详细信息
docker logs <Container ID/name>
```

## Docker Storage - Volumes

```
docker volume create data_volume  # /var/docker/volumes
docker run –v data_volume:/var/lib/mysql mysql
docker run –v /data/mysql:/var/lib/mysql mysql
docker run --mount type=bind, source=/data/mysql, target=/var/lib/mysql mysql
```

![](https://s2.loli.net/2024/02/01/DzsRp1BOh4SrnyM.jpg)

## 🍙 Docker build / Create an image - Docker File

```dockerfile
# Start from a base OS or another image
FROM Ubuntu  
# Install all dependencies
RUN apt-get update
RUN apt-get install python
RUN pip install flask
RUN pip install flask-mysql
# Copy source code
COPY . /opt/source-code
# Specify Entrypoint
ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
```

```bash
docker build -t <image-name>:<tag> .
```

## Push to docker hub

```bash
docker login
docker build -t <usrname>/<container-name> .
docker push <usrname>/<container-name>
```

## Environment Variables

> [!note] Python 获取环境变量的方法
>
> ```python
> import os
> print(os.environ.get("APP_COLOR"))
> ```

```
docker run -e APP_COLOR=blue simple-webapp-color
```

可以用 `docker inspect <ID/name>` 查看 docker 的所有环境变量

## Docker Compose

编写 `docker-compose.yaml` 来将关联的容器进行组合。一条命令即可创建并启动所有服务。

```
docker compose up
```

---

> [!question] 查看一个 image 是基于什么 Linux 系统的？
>
> ```
> $ docker run python:3.6 cat /etc/*release*
> ```

---

[Docker Training Course for the Absolute Beginner | KodeKloud](https://kodekloud.com/courses/docker-for-the-absolute-beginner)
