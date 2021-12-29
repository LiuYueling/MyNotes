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


