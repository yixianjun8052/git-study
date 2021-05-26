# git

```
# 初始化：
	git init

# 查看日志：
	git log
	git reflog

# 查看配置信息：
	git config -l

# 查看系统配置信息（‪D:\Program Files\Git\etc\gitconfig）：
	git config --system --list

# 查看全局配置信息：
	git config --global --list
	
# 配置用户标识（‪‪C:\Users\yi\.gitconfig）：
	git config --global user.name username
	git config --global user.email email

# 查看状态：
	git status

# 添加到暂存区：
	git add

# 放弃文件到暂存：
	git restore --staged file

# 提交到本地库：
	git comit -m "comment" file

# 放弃修改：
	git restore file

# 切换版本：
	git reset --hard 版本号

分支
# 查看分支：
	git branch -v

# 创建分支：
	git branch 分支名

# 删除分支：
	git branch -d 分支名

# 切换分支：
	git checkout 分支名

# 合并指定分支到当前分支：
	git merge 分支名

冲突合并：两个分支在同一文件的同一处修改会产生冲突，解决冲突是编辑文件后再添加提交，提交不写文件名。

远程库
# 远程库起别名：
	git remote add 别名 远程库HTTPS:URL/SSH

# 查看远程库别名：
	git remote -v

# 推送到远程库：
	git push 别名/远程库HTTPS:URL/SSH 分支名

# 拉取到本地库：
	git pull 别名/远程库HTTPS:URL/SSH 分支名

# 克隆到本地库：
	git clone 远程库HTTPS:URL
```

SSH公钥免密登录

```
# 1.生成本地SSH密钥（C:\Users\yi\.ssh）-C 注释：
	ssh-keygen -t rsa -C yixianjun8052@163.com
	
# 2.在github/gitee个人账户上设置SSH公钥保存。
```

# SVN

服务端：SVNBucket、VisualSVM

客户端：TortoiseSVN

常用操作：检出Checkout、提交Commit、更新Update，撤销还原Revert，忽略ignore，冲突，主干trunk分支branches的创建、分离、合并Merge、切换Switch、代码暂存Shelve、复杂合并Beyond Compare
