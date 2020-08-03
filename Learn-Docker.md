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

### 1.3  结合阿里云操作

* 将镜像推送到Registry

  `docker login --username=叫我熊咚咚 registry.cn-hangzhou.aliyuncs.com`

  `docker tag [ImageId] registry.cn-hangzhou.aliyuncs.com/xiongdongdong/ubuntu_xiong:[镜像版本号]`

  `docker push registry.cn-hangzhou.aliyuncs.com/xiongdongdong/ubuntu_xiong:[镜像版本号]`
### 1.4 `Dockerfile`创建镜像

`docker commit`来扩展一个镜像比较简单，但不方便在一个团队中共享。我们可以使用`docker build`来创建一个新的镜像，为此需要先创建一个`Dockerfile`，包含一些如何创建镜像的命令。

`Dockerfile`仅仅是用来制作镜像的源码文件，是构建容器过程中的指令，`docker`能够读取`Dockerfile`的指令进行自动构建容器，基于`Dockerfile`制作镜像，每一个指令都会创建一个镜像层，即镜像都是多层叠加而成的。因此层越多效率越低，创建镜像层越少越好，能在一个指令完成的动作尽量通过一个指令定义。

`Dockerfile`的基本语法是使用`#`来注释，`FROM`指令告诉`docker`使用哪个镜像作为基础，接着是维护者的信息，`RUN`开头的指令会在创建中运行。编写完成`Dockerfile`后可以使用`docker bulid`来生成镜像。`build`进程要做的第一件事情就是上传这个`Dockerfile`内容，因为所有的操作都要依据`Dockerfile`来进行。然后`Dockerfile`中的指令被一条一条的执行。每步都创建了一个新的容器，在容器中执行指令并提交修改。当所有指令都执行完毕之后，返回了最终的镜像`id`，所有的中间步骤所产生的容器都被删除和清理了。