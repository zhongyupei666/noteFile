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



#### 2.1.1、`git init`的基本操作：

> 如果初始化成功，执行了`git init`命令的目录下就会生成`．git`目录。这个`．git`目录里存储着管理当前目录内容所需的仓库数据。

我们在操作做的时候，要学会使用`git status`来对我们的`git`仓库进行一个状态的查看；



#### 2.1.2、`git commit` 的操作指令：

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



`git diff` 指令：`git diff`命令可以查看工作树、暂存区、最新提交之间的差别。



### 2.2、`git` 中的分支操作：

显示所有的分支，并展示当前所在的分支的操作：

`git branch`



以当前的分支为基础，创建新的分支，并执行切换的操作：

`git checkout -b`



当然，如我们所想见到的那样，我们也可以使用`git branch + BranchName`来创建一个分支；

```bat
PS E:\NoteFile> git branch
* main
PS E:\NoteFile> git branch leftBranch
PS E:\NoteFile> git branch
  leftBranch
* main
PS E:\NoteFile> git checkout leftBranch
Switched to branch 'leftBranch'
M       "github\345\255\246\344\271\240/1\343\200\201git\344\270\212\347\232\204\344\270\200\344\272\233\345\237\272\347\241\200\351\205\215\347\275\256.md"
PS E:\NoteFile> git branch
* leftBranch
  main
PS E:\NoteFile>
```

在这个状态下像正常开发那样修改代码、执行`git add`命令并进行提交的话，代码就会提交至feature-A分支

#### 实验:

【实验内容】：

下面我们在分支`leftBranch`中新加一个文件`readme.md`的时候再切换回`master`分支，查看会发生什么情况；

【实验步骤】：

```bat
PS E:\NoteFile> ls


    目录: E:\NoteFile


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2024/1/21     13:46                github学习
d-----         2024/1/20     23:11                git学习
d-----         2024/1/20     22:08                windows
d-----         2023/10/4     21:39                技术随笔笔记
d-----         2023/9/23     20:07                编程语言笔记汇总
d-----         2023/9/10     17:59                网络安全
d-----         2023/9/10     18:21                网络工程笔记
-a----         2024/1/21     13:47              0 readme.bat


PS E:\NoteFile> git status
On branch leftBranch
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   "github\345\255\246\344\271\240/1\343\200\201git\344\270\212\347\232\204\344\270\200\344\272\233\345\237\272\347\241\200\351\205\215\347\275\256.md"

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        readme.bat

no changes added to commit (use "git add" and/or "git commit -a")
PS E:\NoteFile> git add .
PS E:\NoteFile> git commit -m "add readme.md"
[leftBranch e46d7d1] add readme.md
 2 files changed, 93 insertions(+), 2 deletions(-)
 create mode 100644 readme.bat
 PS E:\NoteFile> git checkout main
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)
PS E:\NoteFile> git branch
  leftBranch
* main
PS E:\NoteFile> ls


    目录: E:\NoteFile


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2024/1/21     13:52                github学习
d-----         2024/1/20     23:11                git学习
d-----         2024/1/20     22:08                windows
d-----         2023/10/4     21:39                技术随笔笔记
d-----         2023/9/23     20:07                编程语言笔记汇总
d-----         2023/9/10     17:59                网络安全
d-----         2023/9/10     18:21                网络工程笔记


PS E:\NoteFile>
```



当如果在使用checkout进行切换的时候，出现这样的报错的时候：

`error: Your local changes to the following files would be overwritten by checkout:`

我们只需要在重新的提交一下仓库即可。



==切回到上一个分支的操作的快捷使用方法：==

`git checkout -`



#### 合并操作：

当我们的分支任务完成结束后，我们可以和主干分支进行合并；

【流程】

先将所有的内容提交到分支上，然后切换回主分支



















