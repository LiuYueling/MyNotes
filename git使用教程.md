# Git 使用教程

[TOC]

## Git 基本用法

基本 bash 命令

| 命令  | 说明             |
| :----- | :---------------- |
| cd *directory* | 进入文件夹       |
| pwd   | 显示当前工作目录 |
| mkdir *directory* | 创建文件夹       |
| rmdir *directory* | 移除空文件夹     |
| rm -rf *directory* | 删除目录中的文件夹以及文件，-r 向下递归，-f 强制执行 |
| cat *file* | 查看文本 |



### 1. 创建 git 仓库

使用 `git init` 可以把当前目录变成 git 可以管理的仓库

当前的目录下会生成一个 .git 的隐藏目录，这个是 Git 来跟踪管理版本的，切勿修改。



### 2. 把文件添加到版本库中

1. 使用 git add \<file\> 添加文件到暂存区

``` bash
$ git add readme.txt #将readme.txt 添加到暂存区
```

2. 使用 git commit 把文件提交到仓库

``` bash
$ git commit -m "提交信息" #将暂存区文件提交到仓库
```

使用 git status 来查看文件状态，是否未提交

``` bash
$ git status
```



### 3. 提交修改

使用 git diff 查看文件修改了什么内容

``` bash
$ git diff readme.txt
```

Git 只能追踪文本文件的改动详情；其他格式的二进制文件只能知道文件改变了，不知道变化了什么。



Git 提交修改与提交文件一样两步 (1. git add ; 2. git commit )。



### 4. 版本回退

使用 git log 查看历史日志

使用 git log --pretty=oneline 显示少量的日志

``` bash
$ git log
$ git log --pretty=oneline
```

使用 git reflog 可以获取到版本号和日志

``` bash
$ git reflog
```



Git 版本回退有两种方法：

1. 使用 `git reset --hard HEAD^` ，如果要回退上上个版本需要把 HEAD^ 改成 HEAD^^ 以此类推。也可以使用 `git reset --hard HEAD~100` 退回指定的版本。
2. 使用 `git reset --hard 版本号 ` 退回指定版本

```bash
$ git reset --hard HEAD^
$ git reset --hard HEAD~100
$ git reset --hard 版本号
```



### 5. Git 工作区和暂存区

**工作区**：就是电脑上看到的目录，持有实际文件，属于工作区范畴。

**版本库(Repostiory)**： 

​	1、暂存区(Index)：缓存区，临时保存你的改动。

​	2、历史区(History)：仓库，存放版本库

### 6. 撤销修改和删除文件

#### 1. 撤销修改

撤销修改的方法：

1. 直接更改文件，然后 add 添加到暂存区，最后 commit 掉；
2. 使用 git reset -- hard HEAD^ 直接恢复到上一个版本。
3. 使用 git checkout -- \<file\> 直接丢弃工作区的修改
4. 使用 git restore 丢弃工作区（建议使用）



使用 git checkout 有两种情况：

1. 文件修改后，没有放到暂存区，使用 git checkout -- file 回到和版本库一模一样的状态。
2. 另外一种是已经放到暂存区，又接着修改，git checkout -- file 就会回到暂存区后的状态。

**注意：** 使用 git checkout --- \<file\> 中 -- 很重要，如果没有 -- 的话，那么命令变成了创建分支。



#### 2. 删除文件

当使用 rm \<file\> 或者直接删除文件，再使用 commit 提交，可以删除版本库中的文件。

在没有 commit 之前，如何恢复文件：

1. 使用 git checkout -- \<file\> 恢复文件
2. 使用 git restore \<file\> 恢复文件



### 7. 远程仓库

#### 7.1. 生成密钥

* 第一步，检查是否已经生成key

  打开 git bash，输入

  ```bash
  ls -al ~/.ssh
  ```

  如果能进入到 .ssh 目录下，则之前已经生成过 .ssh 密钥，可以直接使用里面的密钥。

* 第二步，如果不能进入到 .ssh 目录，则需要生成 SSH 密钥

  1. 检查 user.name 和 user.email 配置

     ```bash
     git config user.name
     git config user.email
     ```

     如果没有配置，则需要创建

  2. 创建 user.name 和 user.email

     ```bash
     git config -global user.name "xxx"
     git config -global user.email "xxx@xxx.xxx"
     ```

  3. 生成密钥

     ```bash
     ssh-keygen -t rsa -C "<user.email>"
     ```

     代码参数含义：

     -t 指定密钥类型，默认是 rsa ，可以省略；

     -C 设置注释文字，比如邮箱；

     -f 指定密钥文件存储文件名。

     

     接着按3个回车，

     ```bash
     [root@localhost ~]# ssh-keygen -t rsa       #<== 建立密钥对，-t代表类型，有RSA和DSA两种
     Generating public/private rsa key pair.
     Enter file in which to save the key (/root/.ssh/id_rsa):   #<==密钥文件默认存放位置，按Enter即可
     Created directory '/root/.ssh'.
     Enter passphrase (empty for no passphrase):     #<== 输入密钥锁码，或直接按 Enter 留空
     Enter same passphrase again:     #<== 再输入一遍密钥锁码
     Your identification has been saved in /root/.ssh/id_rsa.    #<== 生成的私钥
     Your public key has been saved in /root/.ssh/id_rsa.pub.    #<== 生成的公钥
     The key fingerprint is:
     SHA256:K1qy928tkk1FUuzQtlZK+poeS67vIgPvHw9lQ+KNuZ4 root@localhost.localdomain
     The key's randomart image is:
     +---[RSA 2048]----+
     |           +.    |
     |          o * .  |
     |        . .O +   |
     |       . *. *    |
     |        S =+     |
     |    .    =...    |
     |    .oo =+o+     |
     |     ==o+B*o.    |
     |    oo.=EXO.     |
     +----[SHA256]-----+
     
     ```

     在 .ssh 目录下得到了两个文件： id_rsa (私有密钥) 和 id_rsa.pub (共有密钥) 。



#### 7.2. GitHub 添加密钥

1. 登录 GitHub ，进入 setings
2. 在personal setings ，点击进入 SSH and GPG key
3. 创建 New SSH key
4. 在 key 输入框中添加你的 id_rsa.pub 密钥；Title 标题可以自己定义，方便标识。
5. 点击 Add SSH key；
6. 在弹出窗口，输入你的 GitHub 密码，点击确认按钮。

到此 GitHub SSH密钥 添加完成



#### 7.3. GitHub 测试

在 git bash 中输入

```bash
ssh -T git@github.com
```

会让你保存 RSA key，输入 yes 即可。





