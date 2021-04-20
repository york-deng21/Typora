# magento

- ## apache建虚拟主机

  - 在`/etc/apache2/sites-available`文件下新建一个配置文件`test.local.conf`

  - `sudo vim test.local.conf`写入以下代码，注意改配置（ServerName）

        <VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
            # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com
        
        ServerAdmin webmaster@localhost
        DocumentRoot /home/york/www
        ServerName test.local
        
        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn
        
        <Directory "/home/york/www">
                Options Indexes FollowSymLinks
                       AllowOverride None
                Require all granted
        </Directory>
        
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
        
        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
            # after it has been globally disabled with "a2disconf".
            #Include conf-available/serve-cgi-bin.conf
        </VirtualHost>

  - 软连接

    `sudo ln -s /etc/apache2/sites-available/ test.local.conf /etc/apache2/sites-enabled/`

  - 进入`/etc/apache2/sites-enabled/`文件夹，ll查看连接情况

  - 进入`etc`文件夹`sudo vim /etc/hosts`添加映射
  
  - 连接成功后`systemctl restart apache`重启apache服务器

 

## 安装Docker

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

## 安装Docker-compose

- 下载二进制文件

  ```
  sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
  ```

- 将可执行权限应用于二进制文件

  ```
  sudo chmod +x /usr/local/bin/docker-compose
  ```

- 创建软链接

  ```
  sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
  ```

- 测试是否安装成功

  ```
  docker-compose --version
  ```

## 下载环境

- 克隆文件到本地

  `git clone https://github.com/pointline/alpine-dnmp.git `

- 在alpine-dnmp目录下

  - 启动

    ```
    sudo docker-compose up -d
    ```

    **启动时报IPv4错误：**

    - 1.修改内核参数

      ​	vi /etc/sysctl.conf

      ​	net.ipv4.ip_forward=0

      ​	改成

      ​	net.ipv4.ip_forward=1

    - 2.重启network服务

      ​	systemctl restart network

    - 3.重启docker服务(**这一步最重要,如果没有执行的话,还是会报错的**)

      ​	systemctl restart docker

    - 4.重启iptables

      ​	systemctl restart iptables

    - 5.查看docker日志

      ​	docker logs ddns

    **ERROR: for mysql Cannot start service mysql: driver failed programming external connectivity on end**

    - docker ps
    - docker stop [CONTAINER ID]

    **mysql端口占用错误**

    - 卸载之前本地安装的mysql

      - 查看：`dpkg --list|grep mysql`

      - 卸载：`sudo apt-get remove mysql-common`

        ​			`sudo apt-get autoremove --purge mysql-server-5.7`

      - 清除残留数据：`dpkg -l|grep ^rc|awk '{print$2}'|sudo xargs dpkg -P`

  - 停止

    ```
    sudo docker-compose down
    ```

  - 重启

    ```
    sudo docker-compose restart
    ```

  - 查看容器状态

    ```
    sudo docker container ls -a
    ```

  - 默认使用PHP7.2，修改PHP版本

    ```
    #　开启所需的单独PHP版本
    vim core/conf/nginx/conf/conf.d/global.conf
    # 多PHP共存
    # 可在web-conf中的虚拟主机配置文件定义自己的upstream
    ```

## 安装mugento

- 解压mugento压缩包到alpine-dnmp目录www文件的新建文件m234中

- 在web-conf文件下将example.conf复制到文件m234.congf

- vim打开m234.conf文件，将服务器名修改为m234.local,路径修改为html/m234

- 回到alpine-dnmp目录重启服务

  `sudo docker-compose exec nginx sh`

  `ls`：查看配置文件

  `nginx -t`：测试配置文件有没有语法错误

  `nginx -s reload`：重载服务

  `exit`：退出

- 到www目录下，给权限

  `sudo chmod -R 777 m234/`

- 更改域名映射

  `sudo vim /etc/hosts/`

- 进入浏览器输入m234.local，跳转到安装界面

  - 检查配置后点击next
  - 在host输入mysql，密码为123456
  - 回到命令行，输入`sudo docker-compose exec mysql bash`进入mysql
  - 登录到mysql:`mysql -uroot -p`,回车再输入密码
  - `show databases;`：查看有哪些数据库
  - `create database m234 character set utf8;`：建数据库
  - 继续next，设置后台用户名密码为admin、123456abc
  - 继续next，直到install



## 认识mugento

- ### 后台

  - #### SALESA

    - 订单

  - #### CATALOG

    - ###### Products(产品)

      - 可添加产品、过滤产品、设置产品属性

      - 添加产品：

        - 基本属性、产品描述、图片视频、搜索优化、自定义选项、畅销量、设计、计划设计更新、运输、礼品选项、可下载信息

      - 产品属性：

        缩略图、产品名字、类型、属性、SKU、价格、数量、销量、可见性、状态、网站、行为等

    - ###### Categories(分类)

      - **Add Root Category**(添加根分类)
      - **Add Subcategory**(添加子分类)
      - **Content**：添加照片、描述等
      - **Display Settings**：显示模式、锚、产品列表排序方式
      - **Search Engine Optimization**(搜索引擎优化)：url键、元标题、关键词、描述
      - **Products in Category** (产品分类)：id、name、SKU、可见度、状态、价格、位置
      - **Design** ：主题、布局、自定义布局更新、设计应用于产品
      - **Schedule Design Update**(规划更新)

  - #### MARKETING(营销)

    - **Cart price rules(购物车规则)**:
      - id、规则、优惠券、开始时间、截止时间、状态、网站、优先权

  - #### CONTENT

    - ###### Pages：

      - 添加页面、过滤页面
      - 页面属性：
        - 标题、url键、布局、商店视图、状态、创建时间、最近修改、动作

    - ###### Blocks:

      - 添加块
      - 块属性：
        - 标题、标识符、商店视图、状态、创建时间、最近修改、动作

    - ###### Themes:

      - 查看主题

  - #### STORE

    - ###### Configuration

      - 全部、网站、通用、商店邮件地址、联系、公告、内容管理、、高级报告

  - #### SYSTEM

    - ###### Cache Management