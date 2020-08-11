## 3. `Dockerfile`

`docker commit`来扩展一个镜像比较简单，但不方便在一个团队中共享。我们可以使用`docker build`来创建一个新的镜像，为此需要先创建一个`Dockerfile`，包含一些如何创建镜像的命令。

`Dockerfile`仅仅是用来制作镜像的源码文件，是构建容器过程中的指令，`docker`能够读取`Dockerfile`的指令进行自动构建容器，基于`Dockerfile`制作镜像，每一个指令都会创建一个镜像层，即镜像都是多层叠加而成的。因此层越多效率越低，创建镜像层越少越好，能在一个指令完成的动作尽量通过一个指令定义。

`Dockerfile`的基本语法是使用`#`来注释，`FROM`指令告诉`docker`使用哪个镜像作为基础，接着是维护者的信息，`RUN`开头的指令会在创建中运行。编写完成`Dockerfile`后可以使用`docker bulid`来生成镜像。`build`进程要做的第一件事情就是上传这个`Dockerfile`内容，因为所有的操作都要依据`Dockerfile`来进行。然后`Dockerfile`中的指令被一条一条的执行。每步都创建了一个新的容器，在容器中执行指令并提交修改。当所有指令都执行完毕之后，返回了最终的镜像`id`，所有的中间步骤所产生的容器都被删除和清理了。

`dockerfile`一般分为四个部分：基础镜像信息、维护者信息、镜像操作指令和容器启动时执行指令。一开始必须指明所基于的镜像名称，接下来说明维护者信息。后面是镜像操作命令。如`RUN`指令，`RUN`指令将对镜像执行跟随的命令，每运行一条`RUN`指令，镜像添加新的一层，并提交。最后是`CMD`指令，来指定运行容器时的操作命令。

### 3.1 指令

指令的一般格式为`INSTRUCTION arguments`，指令包括`FROM,MAINTAINER,RUN`等。

* `FROM`

  格式为`FROM<image>`或`FROM<image>:<tag>`

  第一条指令必须为`FROM`指令，并且如果在同一个`Dockerfile`中创建多个镜像时，可以使用多个`FROM`指令。

* `MAINTAINER`

  格式为`MAINTAINER<name>`，指定维护者信息。

* `RUN`

  格式为`RUN(command)`或`RUN["executable","param1","param2"]`。

  前者在`shell`终端中运行命令，即`/bin/sh -c`，后者使用`exec`执行。指定使用其他终端可以通过第二种方式实现，例如`RUN["/bin/bash", "-c", "echo hello"]`。每条`RUN`指令将在当前镜像基础上执行指定命令，并提交为新的镜像，当命令较长时可以使用`\`来换行。

* `CMD`

  `CMD ["executable","param1","param2"]`使用`exec`执行，推荐方式；

  `CMD command param1 param2`在`/bin/sh`中执行，提供给需要交互的应用；

  `CMD ["param1","param2"]`提供给`ENTRYPOINT`的默认参数；

  指定启动容器时执行的命令，每个`Dockerfile`只能有一条命令，如果指定了多条命令，只有最后一条会被执行。如果用户启动容器时指定了运行的命令， 则会覆盖掉`CMD`指定的命令。

* `EXPOSE`

  格式为`EXPOSE<port>[<port>...]`

  告诉`Docker`服务端容器暴露的端口号，供互联系统使用，在启动容器时需要通过`-P`，`Dcoker`主机会自动分配一个端口转发到指定的端口。

* `ENV`

  格式为`ENV<key><value>`。指定一个环境变量，会被后续`RUN`指令使用，并在容器运行时保持。

* `ADD`

  格式为`ADD<src><dest>`。

  复制本地主机的`<src>`到容器中的`<dest>`，当使用本地目录为源目录时，推荐使用`COPY`。

* `ENTRYPOINT`

  两种格式：`ENTRYPOINT ["executable", "param1", "param2"]`和`ENTRYPOINT command param1 param2`。

* `VOOLUME`

  格式为`VOLUME ["/data"]`。

* `USER`

  格式为`USER daemon`。指定运行容器时的用户名或`UID`，后续的`RUN`也会使用指定用户。

* `WORKDIR`

  格式为`WORKDIR/path/to/workdir`。

  为后续的`RUN,CMD,ENTRYPOINT`指令配置工作目录。

* `ONBUILD`

  格式为`ONBUILD [INSTRUCTION]`。配置当所创建的镜像作为其它新创建镜像的基础镜像时，所执行的操作命令。
  
### 3.2 创建

  编写完成`Dockerfile`之后，可以通过`docker build`命令来创建镜像。基本格式为`docker build[选项]路径`，该命令将读取指定路径下（包括子目录）的`Dockerfile`，并将该路径下所有内容发送给`Docker`服务端，由服务端来创建镜像。因此一般建议放置`Dockerfile`的目录为空目录。也可以通过`.dockerignore`文件（每一行添加一条匹配模式）来让`Docker`忽略路径下的目录和文件。

