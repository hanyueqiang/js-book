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

