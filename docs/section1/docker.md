# Docker备忘

参考

<https://summer25.net9.org/backend/docker/note/>

从镜像创建一个容器并且运行
```bash
docker run
```

常用的选项:

- `-d/--detach` 后台运行
- `-e/--env <variable>=<value>` 环境变量
- `--rm` 运行完以后删除
- `--name` 命名
- `-p/--publish <host_port>:<container_port>` 端口映射
- `-it`, `--interactive --tty` 交互模式并且分配伪终端（exit退出）

```bash
docker run --rm -it ubuntu
```

从`ubuntu`创建一个容器，并且分配一个伪终端.

```bash
docker build
```

- `-f/--file` 指定构建所用的Dockerfile
- `-t/--tag` 为镜像命名并添加标签

!!! tip 
    这里`.`指的是镜像构建的上下文路径，比如`COPY`就会用到这个上下文
    这里不能用`../`也不能用`/`开头的绝对路径

```bash
docker build -f path/to/Dockerfile -t test:1.0 .
```

列出已有的镜像：

```bash
docker images
```

列出运行的容器;

```
docker ps --all
```

```bash
docker exec <options> <container> <command>
docker attach <options> <container> # 连接到后台容器，^P断开
docker start <options> <container> # 启动停止运行的容器
docker stop
docker rm # 删除容器
docer rmi # 删除镜像
docker cp <options> <container>:<src_path> <dest_path>
docker cp <options> <src_path> <container>:<dest_path>
# 宿主机和容器之间复制文件
```

## Dockerfile例子

写入./Dockerfile

```bash
FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive
WORKDIR /usr/src/cpp
RUN echo -e "build a cpp environment" > test.txt \ 
    && apt update && apt install -y build-essential 

RUN cat << EOF > main.cpp
#include <iostream>
int main() {
    std::cout << "Hello world!" << std::endl;
    return 0;
}
EOF

RUN g++ main.cpp -o main
CMD ["./main"]
```

!!! tip
    docker镜像有分层特性，因此RUN很多东西的时候最好用`&&`连接

!!! tip
    还可以分阶段构建
    ```bash
    # build
    FROM ubuntu AS builder

    WORKDIR /usr/src/cpp

    ENV DEBIAN_FRONTEND=noninteractive

    RUN apt update && apt install -y build-essential

    COPY main.cpp .

    RUN g++ main.cpp -o main -static

    # runtime
    FROM scratch

    COPY --from=builder /usr/src/cpp/main .

    CMD ["./main"]
    ```

build的时候可以传代理

```bash
docker build \
  --build-arg http_proxy=http://host.docker.internal:7890 \
  --build-arg https_proxy=http://host.docker.internal:7890 \
  -t cpp:1.0 .
```

```bash
docker run -rm cpp:1.0
```

这里比较好的操作是在宿主和Dockerfile同目录下写好main.cpp然后Dockerfile里面加上

```bash
COPY main.cpp
```

## 数据

**数据卷**

用`type=volume`，可省略

```bash
docker volume create database
docker volue ls
docker volume inspect database
docker volume rm database
```

```bash
docker run --name web_backend \
    --mount source=database, target=/app/database \
    backend
docker inspect web_backend
```

**主机目录**

用`type=bind`

```bash
docker run --name web_backend \
    --mount source=database,target=/app/database \
    --mount type=bind,source=/usr/apps/web_database,target=/app/database \
    --mount type=bind,source=/usr/apps/web_app/.env,target=/app/.env,readonly \
    backend
```
