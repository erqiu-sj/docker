<!--
 * @Author: 邱狮杰
 * @Date: 2021-01-01 19:48:14
 * @LastEditTime: 2021-01-24 15:44:41
 * @FilePath: /docker/README.md
 * @Description: 描述
-->

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
```

# 容器命令

```shell
docker run [options] image #启动一个镜像
docker rm -f $(docker images -ap) # 递归删除所有容器
docker run -it imageName /bin/bash # 进入容器
exit # 退出容器
docker ps [options] # 查看站在运行的容器
docker rm [options]  容器id # 删除一个容器，但是不能删除in xiang a站在运行的容器

docker start 容器id # 启动一个容器
docker restart  # 重启一个容器
docker stop # 停止当前正在运行的容器
docker kill #  强制停止正在运行的容器，一般是在stop报错后使用
```

# commit 镜像

> docker commit 提交容器成为一个新的副本

```docker
docker commit -m='提交信息' -a='作者' 容器id 目标镜像名:[tag=version]
```

# docker 数据卷

> 总结一句话：为了数据的同步操作和持久化，容器间数据也可以共享的

```docker
docker run -it -v 主机目录地址：容器目录
-e 配置容器环境 [mysql]
-P 大p = 随机映射端口
```

## 具名挂载和匿名挂载

```shell
-v 路径：路径：ro|rw[可读|可写]
# 匿名挂载
docker run  -P -d --name nginx01 -v /etc/nginx(容器内地址) nginx
docker volume --help
[create] 创建一个卷
[inspect] 查看当前卷状态
[ls] 查看当前所有数据卷
[rm] 移除卷
# 如果使用匿名挂载卷名则会是乱码，但这乱码代表了一个linux上目录地址
# 具名挂载

docker run -P -d --name nginx02 -v juming(如果带斜线，则会地址，不带则会卷名):/etc/nginx nginx

# 两种方式都可以使用docker volume inspect 卷名 查看挂载地址


# 注意没有指定目录的卷都在／var/lib/docker/volume路径下，不建议使用匿名挂载

```

# dockerFile

```shell
FROM # 指定运行镜像 nginx
MAINTAINER # 作者名+邮箱
RUN #构建时需要执行的命令
ADD #添加内容，比如nginx上需要vim，就需要vim压缩包，可以自动解压添加tar包这是和COPY命令的区别
WORKDIR #镜像的工作目录 ／bin?
VOLUME #数据卷 [juan,juan]在容器内部，匿名挂载
EXPORT #默认暴露端口
CMD # 执行运行时候输出的命令，CMD echo 'awesome'，只有最后一个会生效，可被替代
ENTRYPOINT #类似CMD，不过不会被替换，会被追加在命令行上
ONBUILD #
COPY # 将文件拷贝到镜像中
ENV #构建时候的配置环境
```

```shell
FROM nginx
MAINTAINER hooks28<1695415918@qq.com>
ENV MYPATH /usr/local
WORKDIR $MYPATH # 启动该容器就会自动进入的目录
RUN yum install -y vim
EXPORT 80
CMD echo $MYPATH
CMD echo '---end---'
CMD /bin/bash
```

```shell
docker build -f dockerfile地址 -t myImages:[tag:version]
# 可以通过docker history 镜像id 查看镜像如何启动的
```
