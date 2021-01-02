# Docker

## 查看所有正在运行的容器

- `docker ps`

## 进入正在运行的容器

- 命令 `docker exec -it 容器id baseshell(/bin/bash)` 进入容器后开启一个新的终端，可以在里面操作

- `docker attach 容器id`进入容器正在执行的终端，当前终端

## 删除全部的容器

- `docker rm -f $(docker ps -aq)`

## 拷贝容器内所有文件到主机

- `docker cp 容器id:容器内路径到 目标主机路径`不能在容器内拷贝

## 给容器起名

- `--name Name`

## 后台运行

- `-d`

## 宿主机：容器内部端口

- `-p 宿主机端口：容器内部端口`

# 帮助命令

```shell
docker info # 显示docker系统信息，包括镜像容器数量
docker 命令 --help
```

# 镜像命令

```shell
docker image [options] # 查看本机上所有镜像
docker pull imageName [options] # 默认拉去最新
docker rm -f 镜像id [镜像id] [镜像id] # 删除镜像
docker rm -f $(docker images -ap) # 递归删除所有镜像
```

# 容器命令

```shell
docker run [options] image #启动一个镜像
docker run -it imageName /bin/bash # 进入容器
exit # 退出容器
docker ps [options] # 查看站在运行的容器
docker rm [options]  容器id # 删除一个容器，但是不能删除站在运行的容器

docker start 容器id # 启动一个容器
docker restart  # 重启一个容器
docker stop # 停止当前正在运行的容器
docker kill #  强制停止正在运行的容器，一般是在stop报错后使用
```
