# Git 使用教程

[TOC]

## Git 基本用法

基本 bash 命令

| 命令  | 说明             |
| :----- | :---------------- |
| cd <directory> | 进入文件夹       |
| pwd   | 显示当前工作目录 |
| mkdir <directory> | 创建文件夹       |
| rmdir <directory> | 移除空文件夹     |
| rm -rf <directory> | 删除目录中的文件夹以及文件，-r 向下递归，-f 强制执行 |
| cat <file> | 查看文本 |



### 1. 创建 git 仓库

使用 `git init` 可以把当前目录变成 git 可以管理的仓库

当前的目录下会生成一个 .git 的隐藏目录，这个是 Git 来跟踪管理版本的，切勿修改。



### 2. 把文件添加到版本库中

1. 使用 git add \<file\> 添加文件到暂存区

    ``` bash
    git add readme.txt #将readme.txt 添加到暂存区
    ```

2. 使用 git commit 把文件提交到仓库

    ``` bash
    git commit -m "提交信息" #将暂存区文件提交到仓库
    ```

使用 git status 来查看文件状态，是否未提交

``` bash
git status
```



### 3. 提交修改

使用 git diff 查看文件修改了什么内容

``` bash
git diff readme.txt
```

Git 只能追踪文本文件的改动详情；其他格式的二进制文件只能知道文件改变了，不知道变化了什么。



Git 提交修改与提交文件一样两步 (1. git add ; 2. git commit )。



### 4. 版本回退

使用 git log 查看历史日志

使用 git log --pretty=oneline 显示少量的日志

``` bash
git log
git log --pretty=oneline
```

使用 git reflog 可以获取到版本号和日志

``` bash
git reflog
```



Git 版本回退有两种方法：

1. 使用 `git reset --hard HEAD^` ，如果要回退上上个版本需要把 HEAD^ 改成 HEAD^^ 以此类推。也可以使用 `git reset --hard HEAD~100` 退回指定的版本。
2. 使用 `git reset --hard 版本号 ` 退回指定版本

    ```bash
    git reset --hard HEAD^
    git reset --hard HEAD^^
    git reset --hard HEAD~100
    git reset --hard <版本号>
    ```



### 5. Git 工作区和暂存区

**工作区**：就是电脑上看到的目录，持有实际文件，属于工作区范畴。

**版本库(Repostiory)**： 

​	1、暂存区(Index)：缓存区，临时保存你的改动。

​	2、历史区(History)：仓库，存放版本库

### 6. 撤销修改和删除文件

#### 6.1. 撤销修改

撤销修改的方法：

1. 直接更改文件，然后 add 添加到暂存区，最后 commit 掉；
2. 使用 `git reset -- hard HEAD^` 直接恢复到上一个版本。
3. 使用 `git checkout -- <file>` 直接丢弃工作区的修改
4. 使用 `git restore` 丢弃工作区（建议使用）



使用 git checkout 有两种情况：

1. 文件修改后，没有放到暂存区，使用 `git checkout -- file` 回到和版本库一模一样的状态。
2. 另外一种是已经放到暂存区，又接着修改，git checkout -- file 就会回到暂存区后的状态。

**注意：** 使用 git checkout --- \<file\> 中 -- 很重要，如果没有 -- 的话，那么命令变成了创建分支。



#### 6.2. 删除文件

当使用 rm \<file\> 或者直接删除文件，再使用 commit 提交，可以删除版本库中的文件。

在没有 commit 之前，如何恢复文件：

1. 使用 git checkout -- \<file\> 恢复文件
2. 使用 git restore \<file\> 恢复文件



### 7. 远程仓库

#### 7.1. 生成密钥

* **第一步**，检查是否已经生成key

  打开 git bash，输入

  ```bash
  ls -al ~/.ssh
  ```

  如果能进入到 .ssh 目录下，则之前已经生成过 .ssh 密钥，可以直接使用里面的密钥。

* **第二步**，如果不能进入到 .ssh 目录，则需要生成 SSH 密钥

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



### 8. 创建与合并分支

每一次提交 git 都把他们串成一条时间线，这一条时间线就是一个分支。

截至目前，只有一条时间线，在 git 里，这条分支叫**主分支**，及master分支。

HEAD 严格来说不是指向提交，而是指向 master ，master 才是指向提交，HEAD 指向的就是当前分支。



首先我们创建 dev 分支，然后切换到dev分支上。

* 创建与切换分支

  ```bash
  git checkout -b dev #创建并切换
  ```

  git checkout 命令加上 -b 参数表示创建并切换，相当于下面两条命令

  ```bash
  git branch dev
  git checkout dev
  ```
  切换分支后可以使用 add 和 commit 提交更改到分支

* 查看分支

  ```bash
  git branch #查看分支，会列出所有的分支，当前分支前面会添加一个星号
  ```

* 合并分支

  需要切换到 master 分支，使用 git merge dev ，把 dev 分支上的内容合并到 master 分支上

  ```bash
  git checkout master
  git merge dev
  ```

  git merge 命令用于合并指定分支到当前分支上。
  
* 删除分支

  ```bash
  git branch -d dev #删除 dev 分支。
  ```



解决冲突

1. 创建分支 fenzhi1 ，在 readme.txt 添加一行内容 8888888，然后提交

   ```bash
   git checkout -b fenzhi1 #创建并切换分支 fenzhi1
   cat readme.txt
   vim readme.txt #添加内容
   cat readme.txt
   git add readme.txt
   git commit -m "在 femzhi1 分支上添加 8888888" #分支 fenzhi1 提交更改
   ```

2. 切换到 master 分支，在 readme.txt 添加 9999999 ，然后提交
   ```bash
   git checkout master #创建并切换分支 master
   cat readme.txt
   vim readme.txt #添加内容
   cat readme.txt
   git add readme.txt
   git commit -m "在 master 分支上添加 9999999" #分支 master 提交更改
   ```
   
3. 合并分支，在 master 分支上合并 fenzhi1 

   ```bash
   git merge fenzhi1
   git status
   cat readme.txt
   ```

   Git 使用 <<<<<<< , ====== , >>>>>>> 标记出不同分支的内容

   其中 <<<<<<<HEAD 指主分支修改的内容，>>>>>>>fenzhi1 指 fenzhi1 上修改的内容                              

   使用 git log 查看分支合并情况



分支管理策略

​		通常合并分支时，git 一般使用 “Fast forward” 模式，在这种模式下，删除分支后，会丢掉分支信息。

​		可以使用参数 `---no-ff  ` 来禁用 “Fast forward” 模式

1. 创建并切换一个 dev 分支

2. 修改并提交 dev 分支 readme.txt 内容

3. 切回主分支(master)

4. 合并dev分支，使用命令 git merge --no-ff -m "注释" dev

5. 查看记录历史

   ```bash
   git checkout -b dev
   vim readme.txt
   git add readme.txt
   git commit -m "add merge"
   git checkout master
   git merge --no-ff -m "merge with no-ff" dev #合并dev分支 --no-ff 表示禁用 fast forward
   git branch -d dev #删除 dev 分支
   git branch
   git log --graph --pretty=oneline --abbrev-commit
   ```

   

**分支策略**：首先 master 主分支应该是非常稳定的，也就是来发布新版本，一般情况不允许在上面干活，干活一般情况下在新建的 dev 分支上干活，干完后，比如要发布，或者说 dev 分支代码稳定后可以合并到主分支 master 上来。



### 9. 现场保存与恢复现场

在研发中，会经常遇到 bug 问题，需要创建 bug 分支修复；

每一个 bug 分支都可以用一个临时分支来修复，修复完成后合并分支，然后将临时分支删除掉。

1. 当前分支的工作没有提交，但现场需要进行保护，先去处理紧急的 issue 分支 ，等处理完毕后回到工作分支恢复现场继续工作。

   ```bash
   git stash #使用 git stash 将当前工作现场隐藏起来，等以后恢复现场后继续工作
   git sataus
   ```

2. 创建分支修复bug，然后合并在发布的分支上，再删除 bug 分支。

3. 恢复现场，继续工作

   切换到工作分支，使用 git stash list 查看隐藏的工作现场

   ```bash
   git stash list
   ```

   使用2个方法可以恢复现场

   1. 使用 git stash apply 恢复，恢复后，stash 内容并不删除，你需要使用命令 git stash drop 来删除。

   2. 使用 git stash pop ，恢复的同时把 stash 内容也删除了。

      ```bash
      git stash apply
      git stash drop
      git stash pop
      ```

      





