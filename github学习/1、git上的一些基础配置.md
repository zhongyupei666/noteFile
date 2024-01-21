[toc]

---

# git 的相关知识

> `git`的相关学习还会在后续的`Git`部分再进行讲解和展示。下面的话，看一下`git` 中的一些简单使用 和 基本的应用。



## 1、`git` 的全局注册：

在使用`git` 的时候需要先对自己的身份进行一个全局的注册，注册的内容有两个，一个是用户的名字，一个是用户的邮箱，对应的注册的指令如下所示：
```bat
git config --global user.name "Jiafeng"
git config --global user.email "2449859587@qq.com"
```

对应的，会在 `~` 目录下创建一个 `.gitconfig`的文件，文件中的内容如下：

```bat
[user]
        name = JiaFeng
        email = 2449859587@qq.com
```

对应的，如果我们想要去修改我们的注册信息的话，直接的修改这个文件即可。

这个只是`git`用来标注你的身份的东西，就像我们在使用一个软件的时候，需要去输入用户名和手机号这样的操作；

==由于在`Github`上公开仓库时，这里的姓名和邮箱地址也会随着提交日志一同被公开，所以请不要使用不便公开的隐私信息；==

顺便一提，将`color.ui`设置为`auto`可以让命令的输出拥有更高的可读性。

对应的代码如下所示：

```bat
git config --global color.ui auto
```

紧接着，我们会在对应的文件中看见多出如下的配置，这里为了表示出增量特性，我们展示出全部的内容：

```bat
[user]
        name = JiaFeng
        email = 2449859587@qq.com
[color]
        ui = auto
```



## 2、`git`实操：

对应的几个模块如下所示：

+ `git` 的基本操作（搭建本地的软件版本管理仓库）
+ 分支操作
+ 更改提交的操作
+ 推送至远程仓库
+ 从远程仓库获取



### 2.1、`git` 的基本操作：

> `git` 的基本操作是需要先搭建一个本地的版本控制平台。这里所需要的流程就是：
>
> ​	`git` 初始化仓库 (`git init` ) --> `git` 将文件保存到暂存区 (`git add `) --> 保存仓库的历史记录 (`git commit`) ； 
>
> 查看仓库的状态：`git status`



### 2.1.1、`git init`的基本操作：

> 如果初始化成功，执行了`git init`命令的目录下就会生成`．git`目录。这个`．git`目录里存储着管理当前目录内容所需的仓库数据。

我们在操作做的时候，要学会使用`git status`来对我们的`git`仓库进行一个状态的查看；



### 2.1.2、`git commit` 的操作指令：

基本的使用：`git commit -m "Description"`

当然，如果想要添加更加的详细的`Description`的话，只需要使用`git commit`即可，然后使用`git`上配置的编辑器来对其进行编辑即可；

在编辑器中对应的语法为：

```txt
●第一行：用一行文字简述提交的更改内容
●第二行：空行
●第三行以后：记述更改的原因和详细内容
并以#（井号）标为注释
```



==终止提交的操作：==

如果在编辑器启动后想中止提交，请将提交信息留空并直接关闭编辑器，随后提交就会被中止

使用`git log`指令查看对应的提交的日志：

```bat
PS E:\NoteFile> git log
rem ----------------------------------
commit 8ff9f4e14395dfec51a9458ec10976776e0cb8f1 (HEAD -> main, origin/main)
Author: JiaFeng <2449859587@qq.com>
Date:   Sun Jan 21 12:48:35 2024 +0800

    2024_01_21
```

`commit`后面的这个值是：指向这个提交的哈希值。

`Author` 后面的内容就是我们在`git config --global user.name/email中设置的内容;`



如果只想让程序显示第一行简述信息，可以在`git log`命令后加上`--pretty=short`。这样一来开发人员就能够更轻松地把握多个提交，会不显示对应提交的日期等信息：

```bat
PS E:\NoteFile> git log
commit 8ff9f4e14395dfec51a9458ec10976776e0cb8f1 (HEAD -> main, origin/main)
Author: JiaFeng <2449859587@qq.com>
Date:   Sun Jan 21 12:48:35 2024 +0800

    2024_01_21
PS E:\NoteFile> git log --pretty=short
commit 8ff9f4e14395dfec51a9458ec10976776e0cb8f1 (HEAD -> main, origin/main)
Author: JiaFeng <2449859587@qq.com>

    2024_01_21
```

















