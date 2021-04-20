# Docker

1. ## 基本概念

   ​	**Docker** 使用 `Google` 公司推出的 Go 语言进行开发实现，基于 `Linux` 内核的 cgroup，namespace，以及 OverlayFS 类的 Union FS等技术，对进程进行封装隔离，属于操作系统层面的虚拟化技术。由于隔离的进程独立于宿主和其它的隔离的进程，因此也称其为容器。

   - **更高效的利用系统资源**

     容器不需要进行硬件虚拟以及运行完整操作系统等额外开销

   - **更快速的启动时间**

   - **一致的运行环境**

   - **持续交付和部署**

     使用 `Docker` 可以通过定制应用镜像来实现持续集成、持续交付、部署。

   - **更轻松的迁移**

     `Docker` 可以在很多平台上运行，无论是物理机、虚拟机、公有云、私有云，甚至是笔记本，其运行结果是一致的。

   - **更轻松的维护和扩展**

     `Docker` 使用的分层存储以及镜像的技术，使得应用重复部分的复用更为容易，也使得应用的维护更新更加简单，基于基础镜像进一步扩展镜像也变得非常简单。

   1. ### 镜像

      - **Docker 镜像** 是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像 **不包含** 任何动态数据，其内容在构建之后也不会被改变。
      - 像只是一个虚拟的概念，其实际体现并非由一个文件组成，而是由一组文件系统组成，或者说，由多层文件系统联合组成。
      - 镜像构建时，会一层层构建，前一层是后一层的基础。每一层构建完就不会再发生改变，后一层上的任何改变只发生在自己这一层。

   2. ### 容器

      - 容器是镜像运行时的实体
      - 实质是进程，容器进程运行于属于自己的独立的命名空间
      - 容器运行时以镜像为基础层，在其上创建一个当前容器的存储层，称为**容器存储层**
      - 文件写入操作应使用**数据卷**，或者**绑定宿主目录**

   3. ### 仓库

      - 一个 **Docker Registry** 中可以包含多个 **仓库**；每个仓库可以包含多个 **标签**；每个标签对应一个镜像。

2. ## 安装

   - 卸载旧版本：

     `sudo apt-get remove docker \`

     ​               		`docker-engine \`

     ​              		 `docker.io`

   - 添加使用HTTPS传输的软件包以及CA证书

     `sudo apt-get update`

     `sudo apt-get install \`

     ​    `apt-transport-https \`

     ​    `ca-certificates \`

     ​    `curl \`

     ​    `gnupg \`

     ​    `lsb-release`

   - 使用国内源：

     `curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`

   - 向`sources.list`中添加Docker软件源

     `echo \`

       `"deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu \`

       `$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`

   - 安装Docker

     更新apt软件包缓存，并安装`docker-ce`

     `sudo apt-get update`

     `sudo apt-get install docker-ce docker-ce-cli containerd.io`

   - 启动Docker

     `sudo systemctl enable docker`

     `sudo systemctl start docker`

   - 建立docker用户组

     - 建立`docker`组：`sudo groupadd docker`
     - 检查当前用户是否在当前用户组：`sudo gpasswd -a $york docker`
     - 将当前用户添加至用户组：`sudo gpasswd -a $USER docker`
     - 更新用户组：`newgrp docker`

   - 测试Docker是否安装正确

     `docker run --rm hello-world`

     输出一大段信息则安装成功

3. ## 使用镜像

   1. ### 获取镜像

      - `docker pull ubuntu:18.04`

   2. ### 运行

      - `docker run -it --rm ubuntu:18.04 bash`
        - `-it`:`-i`:交互式操作，`-t`：终端
        - `--rm`：容器退出后随之将其删除，默认情况不需要
        - `bash`：放在镜像名后的是命令

   3. ### 列出镜像

      - `docker  image ls`:列出已下载的镜像（可以根据仓库名、标签等列出指定镜像）
      - `docker system df`:查看镜像、容器、数据卷所占用的空间
      - **虚悬镜像**：无标签镜像
        - 查看：`docker image ls -f dangling=true`
        - 删除：`docker image prune`
      - `docker image ls -a`：显示包括中间层镜像在内的所有镜像

   4. ### 删除本地镜像

      - `docker image rm 镜像（名、id、摘要）`
      
      - 使用 `docker image ls -q` 来配合使用 `docker image rm`，这样可以成批的删除希望删除的镜像。如删除所有仓库名为redis的镜像：
      
        `$ docker image rm $(docker image ls -q redis)`