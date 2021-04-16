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
  
  - 连接成功后`ystemctl restart apache`重启apache服务器

 

