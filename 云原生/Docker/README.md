# Command

- docker run: 用于在容器中运行一个新的可执行程序或命令。它是使用 Docker 镜像创建和启动容器的主要命令。  
- docker rmi: 用于删除本地的一个或多个 Docker 镜像。rmi 表示 "remove image"，即删除镜像。  
- docker build -t: 用于构建一个新的 Docker 镜像并为其指定一个标签（tag）。-t 表示 "tag"，即为镜像添加标签。  
- docker build -f: 用于指定要使用的 Dockerfile 文件路径或名称。-f 表示 "file"，即指定 Dockerfile。  
- docker --build-arg: 用于在构建 Docker 镜像时传递构建参数（build arguments）。--build-arg 选项允许您在构建过程中向 Dockerfile 中的 ARG 指令传递值。  
- docker push: 用于将本地构建的 Docker 镜像推送（上传）到远程的 Docker 镜像仓库。通过使用 docker push 命令，您可以将自己构建的镜像发布到公共或私有的镜像仓库，以供其他人访问和使用  
- docker ps: 查看正在运行的容器
- docker ps -a: 查看所有容器 



# Dockerfile
- FROM：指定基础镜像，表示构建新镜像所依赖的基础操作系统镜像。  
- RUN：在镜像中执行命令。可以运行各种命令，如安装软件包、执行编译等。每个 RUN 指令都会在新的镜像层中执行，并且可以使用反斜杠（\）进行多行命令的拆分。  
- COPY：将文件从构建上下文复制到镜像中。COPY 指令可以复制本地文件到镜像中的指定路径。源文件可以是文件或目录。
WORKDIR：设置工作目录，即后续指令执行的默认路径。  
- ENV：设置环境变量。可以在后续的指令中使用这些环境变量。
- EXPOSE：声明容器运行时监听的端口号。这只是一个元数据，用于指示容器在运行时可以接受的网络连接。  
- CMD：指定容器启动时要执行的命令。CMD 可以提供默认值，但可以被 Dockerfile 中的其他 CMD 或 ENTRYPOINT 覆盖。在 Dockerfile 中，可以使用多个 CMD 指令，但只有最后一个 CMD 指令会生效。如果在 Dockerfile 中出现多个 CMD 指令，那么只有最后一个 CMD 指令会被执行，之前的 CMD 指令将被忽略。  
- ENTRYPOINT：指定容器启动时要执行的固定命令。ENTRYPOINT 的参数是不可覆盖的，但可以与 CMD 结合使用。
- VOLUME：声明容器中的挂载点，用于持久化存储数据。这使得可以将主机文件系统的目录或其他容器的挂载点与容器内的特定目录进行关联。  
- WORKDIR 是 Dockerfile 中的指令，用于设置工作目录（Working Directory）。它指定了后续指令执行的默认路径。WORKDIR 指令并不会创建实际的目录结构，只是在 Dockerfile 中设置一个工作目录的上下文。如果工作目录不存在，Docker 在执行后续指令时会自动创建该目录  




