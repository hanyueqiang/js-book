## 从代码库clone

	git clone https:///gitbook.git

## clone下来代码到指定文件夹名称(dirname)

	git clone https://gitbook.git dirname

## 本地已存在代码关联到远程代码库

	git init // 初始化一个git仓库

	git remote add origin https://gitbook.git

## 安装git后设置用户名、邮箱

	git config --global user.name "your name" //设置用户名 --global:全局设置

	git congfig --global user.email "your email" //设置邮箱

## 查看用户名、查看邮箱

	git config user.name 

	git config user.email

## git代理设置

	git config --global http.proxy htttp://dev.com:8080 //设置代理

	git config --global --get --global http.proxy //查看代理

	git congfig --global --unset http.proxy //取消代理

	git config --global list //查看设置列表

## 记住密码
1.使用命令：

	git config --global credential.helper store //如果没有--global，会在当前项目下.git/config文件中添加

2.修改配置：

在 Git 的配置文件 .gitconfig 里面会有你先前配好的 name 和 email，只需在下面添加如下代码即可：

	[credential]
     	helper = store

	//Windows 系统文件路径为：C:\Users\Administrator\.gitconfig

	//Linux 下路径为：~/.gitconfig

下次再输入用户名 和密码 时，git 就会记住，从而会在配置文件 .gitconfig 的同级目录下生成一个 .git-credentials 文件， 此文件将会明文储存用户名和密码。

## 查看提交记录 

### 查看某个文件的提交记录

#### 修改还未提交到暂存区的时候：

	git diff <filename>

#### 提交到暂存区之后：

	git diff --cache <filename> 
	或：
	git diff --staged <filename>

#### commit 之后：

	git log -p <filename>

### 查看某人的提交记录

	git log --stat --author=someone

### 查看某次 commit 的提交记录

	git show <commit-hash-id>
	或：
	git log -p <commit-hash-id>

## 查看分支
	git branch //查看本地分支，当前分支前会有*号

	git branch -r //查看远程分支

	git branch -a //查看远程和本地分支

## 创建和切换分支

	git branch <branch-name> //创建新的分支，不会切换到新分支上，本地创建的分支不会提交到远程，对其他人不可见。

	git checkout <branch-name> //切换分支，branch-name 既可以是本地分支也可以是远程分支

	git checkout -b <branch-name> //创建新分支并切换到新分支

	git push origin <branch-name>:<branch-name> //讲新创建的本地分支推送到远程
	
	git fetch 获取远程分支

	git checkout -b branchName origin/branchName //会在本地新建分支并切换到该分支上。

## `develop`分支强制`push`远程

	git push origin develop:master -f  //把本地的 develop 分支强制(-f)推送到远程 master但是上面操作，本地的 master 分支还是旧的，通常来说应该在本地做好修改再去 push 到远端，所以我推荐如下操作

	git checkout master //切换到master分支

	git reset --hard develop //把master重置成develop分支

	git push origin master --force //再推送到远程分支

## 删除分支
	git branch -d <branch-name> //删除本地分支

	git branch -D <branch-name> //此分支没有被merge其他分支，可以强制删除。

## 查看标签

标签总是和commit挂钩，如果commit即出现在master分支又出现dev分支，那么两个分支都可以看见标签；
	
	git tag //标签不是按时间顺序列出，而是字母排列
	
	git show v1.0.1 //查看指定标签详细信息

	git tag v1.0.0 //默认标签打在最新提交的commit上

	git tag v1.0.0 <commit-id> //标签指定commit id

	git tag -a <tagname> -m "hello world..." //指定标签信息
	
	git push origin v1.0.0 //推送某个标签到远程
	
	git push origin --tags //一次性推送全部尚未在远程的本地标签

	git tag -d v1.0.1 //删除本地标签
	
	git push origin :refs/tags/v1.0.1 //如果标签已经推送到远程 先从本地删除，然后从远程删除

## 查看修改和撤销记录

	git diff //简单查看文件修改 还没有add前 希望看下做了哪些修改

	git diff <file-name> //只想检查某个文件做了修改

	git checkout . //撤销修改 
	
	git reset --hard //撤销修改 同上
	
	git checkout <file-name> //撤销修改指定文件
		
	git diff --catch //把文件add后 查看修改

	git reset //撤销修改 同 git checkout .
	
	git reset --hard origin master //commit后撤销修改

	git reset --hard HEAD^ //如果撤销修改的内容已经提交，需要先恢复本地仓库，再强制push远程 
	
	git push -f //强制push远程 慎用
	
	
