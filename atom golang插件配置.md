#Atom编辑器golang开发环境配置
选择atom的原因：
因为atom对git提供非常好的支持，大多数项目都是放在github上或者开源中国上，在使用atom的时候编辑器能够提供git的支持，那些文件被修改过，那些是新建立的文件都会在文件颜色上进行区分，还能在代码上显示那部分代码是修改过的代码，对经常使用git的人来说非常的方便。

好，下面来介绍怎样在atom上安装对golang的开发支持，一下使用的atom版本为：1.7

##go环境的安装
这部分是非常重要的，编辑器是通过调用go工具来完成各种各样的操作提示的，所以我们首先需要安装好所需要的全部go插件

1、 安装golint（build 的错误都会显示出来）
```
go get github.com/golang/lint
go install github.com/golang/lint
```

2、 安装gocode（提供代码补全）
```
go get -u github.com/nsf/gocode
go install github.com/nsf/gocode
```

3、安装godef （代码跳转）
```
go get-v code.google.com/p/rog-go/exp/cmd/godef
go install -v code.google.com/p/rog-go/exp/cmd/godef
```
如何使用：光标在目标代码上的时候，使用快捷键ctrl-k跳转到目标代码的定义代码上
PS：这个插件楼主没有安装成功

到这里其实我觉得就可以进行go开发了，有了代码自动补全、语法检测就够了。如果觉得不够的同学还可以继续安装其他的一些go插件来添加更多的开发功能。

PS：楼主是没有安装下面的这些插件的，刚开始安装了试用了一下就删掉了

其他go插件安装：
1、 安装goimport（包自动管理）
```
go get golang.org/x/tools/cmd/goimports
go install golang.org/x/tools/cmd/goimports
```
安装该插件后编辑器会自动帮你对import包进行管理，自动导入需要的包，自动删除没有使用的包（这点我觉得非常不好用）

2、 安装gometalinter（语法检查）
```
go get github.com/alecthomas/gometalinter
go install github.com/alecthomas/gometalinter
```

3、安装 godef（代码跳转）
```
go get -v code.google.com/p/rog-go/exp/cmd/godef
go install -v code.google.com/p/rog-go/exp/cmd/godef
```

PS：
在安装如上的这些插件时很可能不能正常安装（天朝的局域网）
推荐几个可以下载的地方：

1、golang中国的下载频道中有一个第三方包的下载工具，只要输入地址之后人家会给你提供下载tar包

2、www.gopm.com也可进行下载

将下载后的源代码包放到GOPATH/src目录下，此步骤只能替代go get的步骤，最后还是需要go install来进行安装

#atom安装go-plus插件
打开atom软件点击`file`-`settings`出现settings界面选择`insrall`在搜索框中搜索`go-plus`插件，搜索完成后点击进行安装。

等待安装完成后，程序启动atom程序点击`package`如果能够看到`go`选项者说明安装成功，快打开一个go文件试试，是不是能够自动提示代码补全，如果编写错误保存后是不是有错误提示`^_^`OK恭喜你你可以使用atom开心的进行go开发了。

PS：
安装go-plus插件时需要翻墙（天朝的局域网）
