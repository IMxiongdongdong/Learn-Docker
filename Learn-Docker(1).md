# Docker学习

## 1. 镜像操作
### 1.1 列出镜像

`docker images`

显示本地已有镜像。镜像的`ID`唯一标识了镜像，`TAG`信息用来标记来自同一仓库的不同镜像，如果不指定具体标记，则默认使用`latest`标记信息。


### 1.2  创建新镜像

`docker commit -a "xiongdongdong" -m "my ubuntu" image_id myubuntu:v1 `

`-a`代表作者

`-m`代表提交信息

`myubuntu:v1`代表标签

### 1.3  推送到阿里云

* 将镜像推送到Registry

  `docker login --username=叫我熊咚咚 registry.cn-hangzhou.aliyuncs.com`

  `docker tag [ImageId] registry.cn-hangzhou.aliyuncs.com/xiongdongdong/ubuntu_xiong:[镜像版本号]`

  `docker push registry.cn-hangzhou.aliyuncs.com/xiongdongdong/ubuntu_xiong:[镜像版本号]`
### 1.4 保存/载入镜像

`docker save`/`docker load`

导入时候会导入及其相关的元数据信息（包括标签等）。

### 1.5 移除镜像

`docker rmi`

移除之前要先删掉有关容器断开连接，如果还是不行可以使用`docker rmi -f`强制删除。

***

## 2. 容器操作

### 2.1 新建容器

`docker run`

`docker run -t -i ubuntu bin/bash`

启动一个`bash`终端，允许用户进行交互。其中`-t`选项让`docker`分配一个伪终端并绑定到容器的标准输入上，`-i`让容器的标准输入保持打开。

当利用`docker run`来创建容器时，`docker`在后台运行的标准操作包括：

* 检查本地是否存在指定的镜像，不存在就从公有仓库下载

* 利用镜像创建并启动一个容器

* 分配一个文件系统，并在只读的镜像层外面挂在一层可读写层

* 从宿主主机配置的网桥接口中桥接一个虚拟接口到容器中去

* 从地址池配置一个ip自动hi给容器

* 执行用户指定的应用程序

* 执行完毕后容器被终止

### 2.2 终止容器

`docker stop`

`docker ps`看到的是正在运行的容器，`docker ps -a`看到包括正在运行和已经终止的容器。

### 2.3 启动已终止容器

`docker start`会启动处于终止状态的容器。

`docker restart`会将一个运行的容器终止在重新启动。

如果要重新进入已启动容器要执行`docker exec`命令。

### 2.4 守护态运行

`docker run -d`让`docker`容器在后面以守护态形式运行。

要获取容器的输出信息可以通过`docker logs`。

### 2.5 导出/导入容器

`docker export/import`

`docker export`可以导出容器快照到本地文件，`docker import`可以从容器快照文件中在导为镜像，也可以通过指定URL或者某个目录来导入。

### 2.6 删除容器

`docker rm`

`docker rm -f`可以强制删除容器
