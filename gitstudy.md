# git

初始化：git init
查看日志：git log，git reflog
配置用户标识：git config --global user.name username
							git config --global user.email email
查看状态：git status
添加到暂存区：git add
放弃文件到暂存：git restore --staged file
提交到本地库：git comit -m "comment" file
放弃修改：git restore file
切换版本：git reset --hard 版本号

分支
查看分支：git branch -v
创建分支：git branch 分支名
删除分支：git branch -d 分支名
切换分支：git checkout 分支名
合并指定分支到当前分支：git merge 分支名
冲突合并：两个分支在同一文件的同一处修改会产生冲突，解决冲突是编辑文件后再添加提交，提交不写文件名。

远程库
远程库起别名：git remote add 别名 远程库url
查看远程库别名：git remote -v
推送到远程库：git push 别名/远程库HTTPS:URL/SSH 分支名
拉取到本地库：git pull 别名/远程库HTTPS:URL/SSH 分支名
克隆到本地库：git clone 远程库HTTPS:URL

SSH公钥免密登录
1.生成本地SSH密钥（C:\Users\yi\.ssh）：ssh-keygen -t rsa -C yixianjun8052@163.com
2.在github/gitee个人账户上设置SSH公钥保存。

```
# 查看配置信息：
git config -l

# 查看系统配置信息：‪D:\Program Files\Git\etc\gitconfig
git config --system --list

# 查看全局配置信息：
git config --global --list

# 修改配置文件，配置用户名和邮箱：‪‪C:\Users\yi\.gitconfig
	git config --global user.name "username"
	git config --global user.email "email@xx.com"
	
# 
```

# SVN

服务端：SVNBucket	客户端：TortoiseSVN
常用操作：检出Checkout、提交Commit、更新Update，撤销还原Revert，忽略ignore，冲突，主干trunk分支branches的创建、分离、合并Merge、切换Switch、代码暂存Shelve、复杂合并Beyond Compare

svn命令

```
# 代码检出，checkout也可以简写为co，这个命令会把服务器上的代码同步到我们电脑上
svn checkout svn://svnbucket.com/xxx/xxx
# 更新代码，执行此命令后会把其他人提交的代码全部更新到我们自己电脑上，update也可以简写为up
svn update
# 提交代码，commit可以简写为ci，-m参数后面跟的是本次提交的描述内容
svn commit -m "提交描述"
# 添加新文件到版本库，只是标记了添加到版本库，我们还需要执行提交命令这个文件才会提交到服务器上
svn add filename
# 添加当前目录下所有php文件
svn add *.php
# 递归添加当前目录下的所有新文件
svn add . --no-ignore --force
# 查看指定文件的所有log
svn log test.php
# 查看指定版本号的log
svn svn log -r 100
# 撤销本地文件的修改（还没提交的）
svn revert test.php
svn revert -r 目录名
# 撤销目录下所有本地修改
svn revert --recursive 目录名
# 查看当前工作区的所有改动
svn diff
# 查看当前工作区test.php文件与最新版本的差异
svn diff test.php  
# 指定版本号比较差异
svn diff -r 200:201 test.php  
# 查看当前工作区和版本301中bin目录的差异
svn diff -r 301 bin
# 查看当前工作区的状态
svn status
# 查看svn信息
svn info
# 查看文件列表，可以指定-r查看，查看指定版本号的文件列表
svn ls 
svn ls -r 100
# 显示文件的每一行最后是谁修改的（出了BUG，经常用来查这段代码是谁改的）
svn blame filename.php
# 查看指定版本的文件内容，不加版本号就是查看最新版本的
svn cat test.py -r 2
# 清理，这个命令我们经常在svn出现报错时可以执行一下，这样就会清理掉本地的一些缓存
svn cleanup
# 若想创建了一个文件夹，并且把它加入版本控制，但忽略文件夹中的所有文件的内容
svn mkdir spool 
svn propset svn:ignore '*' spool 
svn ci -m 'Adding "spool" and ignoring its contents.'
# 若想创建一个文件夹，但不加入版本控制，即忽略这个文件夹
svn mkdir spool 
svn propset svn:ignore 'spool' . 
svn ci -m 'Ignoring a directory called "spool".'
# 切换当前项目到指定分支。服务器上更新新版本我们经常就用这个命令来把当前代码切换到新的分支
svn switch svn://svnbucket.com/test/branches/online1.0
# 重定向仓库地址到新地址。如果你的svn地址变了，不需要重新checkout代码，只需要这样重定向一下就可以了。
svn switch --relocate 原svn地址 新svn地址
# 创建分支，从主干创建一个分支保存到branches/online1.0
svn cp -m "描述内容" http://svnbucket.com/repos/trunk http://svnbucket.com/repos/branches/online1.0
# 合并主干上的最新代码到分支上
cd branches/online1.0
svn merge http://svnbucket.com/repos/trunk 
# 分支合并到主干
svn merge --reintegrate http://svnbucket.com/repos/branches/online1.0
# 删除分支
svn rm http://svnbucket.com/repos/branches/online1.0
# 查看SVN帮助
svn help
# 查看指定命令的帮助信息
svn help commit
```
