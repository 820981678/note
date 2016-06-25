# Git本地与远程仓库交互
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

如出现`git remote add origin`错误提示，需要使用命令先删除掉仓库：
```
git remote rm origin
```

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

## 6.更新远程仓库代码到本地
Git中从远程的分支获取最新的版本到本地有这样2个命令：

### git fetch
相当于是从远程获取最新版本到本地，不会自动merge
```
git fetch origin master
git log -p master..origin/master
git merge origin/master
```
以上命令的含义：

首先从远程的origin的master主分支下载最新的版本到origin/master分支上
然后比较本地的master分支和origin/master分支的差别
最后进行合并
上述过程其实可以用以下更清晰的方式来进行：
```
git fetch origin master:tmp
git diff tmp 
git merge tmp
```
从远程获取最新的版本到本地的test分支上，之后再进行比较合并

### git pull
相当于是从远程获取最新版本并merge到本地
```
git pull origin master
```
上述命令其实相当于git fetch 和 git merge
在实际使用中，git fetch更安全一些
因为在merge前，我们可以查看更新情况，然后再决定是否合并

### 强制覆盖本地内容
本地有修改和提交，如何强制用远程的库更新更新。我尝试过用git pull -f，总是提示 You have not concluded your merge. (MERGE_HEAD exists)。

我需要放弃本地的修改，用远程的库的内容就可以，应该如何做？傻傻地办法就是用心的目录重新clone一个，正确的做法是什么？

正确的做法应该是：
```
git fetch --all
git reset --hard origin/master
```
git fetch 只是下载远程的库的内容不做任何的合并，git reset 把HEAD指向刚刚下载的最新的版本。
