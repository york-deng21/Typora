* #### 创建版本库

  * git init：将当前目录变成Git仓库（初始化仓库）
  * git add ***：将文件添加到仓库（将每次add的修改放到暂存区）
  * git commit -m "***"：将文件提交到仓库，后面跟说明（提交暂存区的内容）
  * git status：查看当前仓库状态

* #### 修改内容

  * git diff：查看修改的内容

  * git log：查看提交记录

    

  * git reset --hard HEAD^：回退一个版本（几个^代表几个版本）

  * git reset --hard commit id：到指定版本

    

  * git checkout(restore) -- ***：丢弃工作区的修改(回退到最后一次修改前)

  * git reset HEAD ***：丢弃缓存区的修改

    

  * git rm：删除文件（删除后要提交）

  * git checkout -- ***：误删后用版本库里的版本替换工作区的版本

* #### 远程仓库

  * ##### 创建SSH Key

    * ssh-keygen -t rsa -C "邮件地址"，一直回车，不用设置密码
    * 在用户主目录里找到.ssh目录，有id_rsa(私钥)、id_rsa.pub(公钥)
    * 登录GitHub，打开“Account settings”、"SSH Keys"
    * 点击add SSH Key,粘贴id_rsa.pub的内容，点击addkey

  * ##### 添加远程库

    * 在GitHub上新建仓库
    * 在本地仓库运行命令git remote add origin git@github.com:GitHub仓库名/本地仓库名.git：与远程库关联
    * git push -u origin master：将本地仓库的内容推送到远程库（第一次提交要加-u）
    * git remote -v：查看远程库信息
    * git remote rm 远程仓库名：删除远程库

  * 克隆远程库

    * ```
      git clone git@github.com:远程库名/本地文件名.git:克隆到本地
      ```

* #### 分支管理

  * git checkout -b dev：创建并切换到dev分支
  * git branch：查看当前分支
  * git switch master：切回master
  * git merge dev：将dev分支合并到master上
  * git branch -d dev：删除dev分支
  * git stash：存储当前工作区
  * git stash list：查看存储的工作区
  * git stash pop：恢复工作区并删除stash
  * git branch -D 分支名：强行删除未被合并的分支

* #### 多人协作

  * 首先，可以试图用`git push origin <branch-name>`推送自己的修改
  * 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并
  * 如果合并有冲突，则解决冲突，并在本地提交
  * 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功
  * 如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

* #### 标签

  * git tag v1.0：给分支打上标签
  * git tag：查看所有标签
  * git tag v1.1 commit id：给对应的commit id打上标签
  * git tag -a v1.2 -m "说明文字"：创建带有说明的标签

  

  * git tag -d v1.0：本地删除标签
  * git push origin.v1.0：推送某个标签到远程（tags：全部标签）
  * git push origin :refs/tags/v1.0：从远程删除标签，但先要从本地删除



* #### 同时push到多个github仓库

  * 创建b账号的ssh：ssh-keygen -t rsa -f ~/.ssh/id_rsa_b -C "youmail@example.com"
  * 将秘钥设置到b账号中
  * 配置config:
    * ![image-20210325122627180](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210325122627180.png)
    * 测试连接是否成功![image-20210325122743723](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210325122743723.png)
  * 在项目文件中配置用户名和邮箱![image-20210325122845012](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210325122845012.png)
  * 修改远程仓库地址![image-20210325123036348](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210325123036348.png)
  * push测试