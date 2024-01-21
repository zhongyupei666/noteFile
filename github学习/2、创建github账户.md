[toc]

---



# 创建账户：



## 1、使用`ssh key`的时候出现的问题排错：

### 1.1 基本的操作流程：

==按照流程来：==

点击`github`上的 Settings 。然后，选择 `SSH and GPG keys`选项。最后，在`SSH keys`上选择 `New SSH key`，然后将生成的公钥信息复制上去即可。

==创建ssh公钥：==

打开终端。输入 `ssh-keygen -t rsa -C "email_address"` 然后，回车按照提示的要求输出密码即可；

==检验成功：==

`ssh git@github.com`

当出现：

`Hi gays! You've successfully authenticated, but GitHub does not provide shell access.`

便意味着成功了。

### 1.2、出现的问题：

当然，不是每次都会成功的，当我们出现如下的问题的时候：
`Error: Permission denied (publickey)`

**根据官方给出的排错思路是：**

先使用 `ssh -vT git@github.com`
查看对应的port是否为22? 如果是的话，下面查看是否是代理出现问题（踩过坑）；
如果对应的ssh-agent没有开启的话，会报Permission denied的问题;
查看：`Get-Service -Name ssh-agent`
若对应的输出为：

```bat
Status   Name               DisplayName
Stopped  ssh-agent          OpenSSH Authentication Agent
```

我们发现就是代理出现了问题，我们下面使用如下的指令来进行代理的设置：

```bat
Get-Service -Name ssh-agent | Set-Service -StartupType Manual
Start-Service ssh-agent
```


然后检查，`ssh-add -l` 是否将对应的私钥加载到ssh中了：
若输出的结果为：
`The agent has no identities.`
那么，下面我们再将对应的私钥添加载到ssh中：
`ssh-add /path`
然后，再次检查` ssh-add -l `查看私钥是否被加载到ssh中，若此时已经由字符串显示，则加载成功，后面在使用：
`ssh -T git@github.com`
就会出现：
`Hi gays! You've successfully authenticated, but GitHub does not provide shell access.`

这也就标志着我们的成功；



**但是！！！所有的结论真的向我们所想的这样吗？？？？**

下面来做实验，检查一下：



## 2、实验：

> + 基础知识理解：
>   + `Get-Service`：获取当前的服务的状态；
>   + `Set-Service`：设置当前的服务的状态；
>   + `Start-Service`：开启服务；
>   + `Stop-Service`：关闭服务；

关于对应的指令的内容，给出如下的官网说明https://learn.microsoft.com/zh-cn/powershell/module/microsoft.powershell.management/set-service?view=powershell-7.4



### 2.1、`ssh-agent`必须要开吗？

【实验步骤】：

​	当我的`github`上只有一个公钥的时候，如果我不开启`ssh-agent`的话，是否正常工作？

【实验操作】：

```bat
PS C:\WINDOWS\system32> Stop-Service ssh-agent
PS C:\WINDOWS\system32> Get-Service -Name ssh-agent

Status   Name               DisplayName
------   ----               -----------
Stopped  ssh-agent          OpenSSH Authentication Agent

PS C:\WINDOWS\system32> ssh-add -l
Error connecting to agent: No such file or directory
PS C:\Users\24498> ssh -T git@github.com
Enter passphrase for key 'C:\Users\xxxx\.ssh/id_ecdsa':			// 输入创建文件的密码即可;
Hi gays! You've successfully authenticated, but GitHub does not provide shell access.
```

**解析一下这个`ssh-agent`的功能**

再去，搜索一下`ssh-agent`的的作用：

```txt
ssh 推荐的登录方式是使用私钥登录。但是如果生成私钥的时候，设置了口令/密码（`passphrase`），每次登录时需要输入口令也很麻烦。可以通过 `ssh-agent` 来管理私钥，把私钥加载进内存，之后便不用再输入密码。
```

==但是这个只是，这个东西的大致描述。继续深挖：==

```txt
用户 Bob 使用 ssh-agent 来管理私钥之后。ssh-agent 会启动一个进程在内存里保存这些私钥。之后每次登录时，ssh 客户端都会跟 ssh-agent 请求是否有目标主机的私钥；如果有，ssh 客户端便能直接登录目标主机。
```

可以了，可以开始总结了：

我们将所有的私钥添加到代理中（`ssh-add /path`），`ssh`代理用自己手中已有的一堆的私钥来解密被公钥加密的密文，挨个试。知道成功，当成功的时候，便可以`ssh`连接上。否则，报错；所以，`ssh-agent`充当的就是一个钥匙管家的作用。所以，他真的必须吗？不是必须的，只能说帮助我们方便了生活。

当然，此处是由于本地只有这一个这样的私钥的，当存在多个私钥的时候，需要使用`ssh`指令中的`-i`参数来指定对应的私钥文件（就是自己找钥匙，而不是让代理来一个一个的找）；



> ==插曲：==
>
> 私钥和公钥，简而言之，私钥就是钥匙，公钥就是锁。但是这个锁非常奇怪，有一个特性，我可以知道这把锁的样子，但是永远不能根据锁来制成解这把锁的钥匙，并且这把锁可以被复制，但是，就是无法制作钥匙。



所以，上述第一个部分所说的遇到的问题，就是在`Enter passphrase for key 'C:\Users\24498/.ssh/id_ecdsa':`这里密码输入错误，所导致的。而为啥在配置完`ssh-agent`之后就不需要输入呢？因为这个就是免密登录的一个玩法，也就是`ssh-agent`的一个好处。

```bat
PS C:\WINDOWS\system32> ssh -T git@github.com
Hi gays! You've successfully authenticated, but GitHub does not provide shell access.
```



## 3、总结：

学习的过程，总会被事物的外表所迷惑。所以，一定要多做实验，多看不同的人的文章以求搞懂自己在实验过程中使用的所有的技术。