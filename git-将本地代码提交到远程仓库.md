# Git将本地代码提交到远程仓库
如何将自己的本地代码库提交到远程Git仓库中，如：github、oschina等远程git代码仓库。

## 1. 初始化本地仓库
跳转到本地代码库目录，输入如下git命令：
```
git init
```
完成以上命令后在本地代码库目录中会生成一个名为`.git`的隐藏文件夹，该文件夹主要用于记录git信息。

## 2.设置远程仓库地址
使用`git remote add origin`命令来设置远程仓库地址
```
git remote add origin https://git.oschina.net/happiness/note.git
```
其中`https://git.oschina.net/happiness/note.git`为远程仓库`https`地址。

## 3.提交代码到本地仓库
查看本地需要提交的文件状态，使用如下命令：
```
git status
```
将我们需要提交的代码文件先添加到本地缓存中，通过`git add .`来添加全部文件，通过`git add filename`来添加单个文件，使用命令如下：
```
git add .

git add filename
```
接下来将缓存中文件提交到本地代码库中，使用`git commit`命令，命令如下：
```
git commit -m "本次提交的说明"
```
完成如上步骤后就将文件提交到了本地代码仓库中了。

## 4.提交到远程仓库
通过`git push`命令来将本地代码库中的文件提交到远程仓库中，命令如下：
```
git push origin master
```
其中`master`为远程仓库分支，我们以`master`主分支为例，就这样等待本地仓库文件的上传，上传完成后就完成了将文件提交到远程仓库。接下来去你的远程仓库（github、oschina）中查看对应的仓库是否已经添加了我们刚刚上传的文件。

## 5.错误处理
在操作第4步骤的时候可能会出现异常，异常代码如下：
```
$ git push origin master
To https://git.oschina.net/happiness/market-update.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://git.oschina.net/happiness/market-update.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
出现错误的主要原因是github中的README.md文件不在本地代码目录中，可以通过如下命令进行代码合并【注：pull=fetch+merge]，命令如下：
```
git pull --rebase origin master
```
执行上面代码后可以看到本地代码库中多了README.md文件，然后在执行第4步骤的命令将本地代码库提交到远程仓库中。
