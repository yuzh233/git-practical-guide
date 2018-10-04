# Git 实用指南

<!-- TOC -->

- [Git 实用指南](#git-实用指南)
    - [版本控制系统（VCS）](#版本控制系统vcs)
        - [版本控制：最基本的功能](#版本控制最基本的功能)
        - [主动提交机制：VCS 与普通文本编辑器的区别](#主动提交机制vcs-与普通文本编辑器的区别)
        - [中央仓库：多人合作的同步需求](#中央仓库多人合作的同步需求)
        - [中央式版本控制系统](#中央式版本控制系统)
    - [分布式版本控制系统（DVCS）](#分布式版本控制系统dvcs)
    - [快速上手](#快速上手)
    - [本地初始化仓库并推送到 GitHub](#本地初始化仓库并推送到-github)
    - [关联 GitHub 与本机的 SSH Key](#关联-github-与本机的-ssh-key)
    - [git pull](#git-pull)
    - [HEAD / master / branch 理解](#head--master--branch-理解)
    - [branch xx / checkout branch xx / checkout -d xx / branch -d xx](#branch-xx--checkout-branch-xx--checkout--d-xx--branch--d-xx)

<!-- /TOC -->

## 版本控制系统（VCS）

**版本控制** / **主动提交** / **中央仓库** 构成了一个最核心的版本控制系统。

### 版本控制：最基本的功能

版本控制系统最基本的功能是版本控制。版本控制，简单的理解就是在文件中的修改历程中保存修改历史，我们可以方便的撤销之前对文件的修改。

在普通文本编辑器中，我们可以使用 Undo 操作回退到上一次的操作；在程序编码，我们可以通过 VCS 回退到指定的一次操作，而不仅仅是上一次操作。

### 主动提交机制：VCS 与普通文本编辑器的区别

使用普通文本编辑器的时候，一次保存就是一次改动，对版本的 `控制` 仅仅是回退到上一次操作。而正常情况下，我们的程序代码修改的生命周期十分长，一次代码的修改，在几天后、几个月后、甚至几年后都可能被翻出来。此时像普通编辑器的“自动保存提交”的功能在对历史代码审查、回退中会变得非常繁琐和无章可循。所以和普通文本编辑器的“撤销”功能不同，VCS 保存修改历史，使用 `主动提交改动` 的机制。

所谓 `主动提交改动` ，是指每次代码的修改和保存不会自动提交，需要手动提交（commit）到仓库，VCS 会把这次提交记录到版本历史中，当往后需要回退到这个版本，可以在 VCS 的历史提交纪录中找到这条记录。

### 中央仓库：多人合作的同步需求

中央仓库作为代码的存储中心，所有人的改动都上传到这里，所有人都可以看到并下载别人上传的改动。

版本控制 / 主动提交 / 中央仓库 这三个要素，共同构成了版本控制系统 VCS 的核心：开发团队中的每个人向中央仓库中主动提交自己的改动和同步别人的改动，并在需要的时候查看和操作历史的版本，这就是版本控制系统。

### 中央式版本控制系统

最基本的模型是：在一台服务器中初始化一个中央仓库，每个人从中央仓库下载初始版本开始并行开发，提交各自的代码到中央仓库并更新其他人的代码同步到自己的机器上。

团队中的每个人需要做的就是：1. 第一次加入团队，从中央仓库取代码到本地； 2. 写好的新功能提交到中央仓库； 3. 同事有新的代码提交，及时同步到本地。实际开发中当然还会经常需要处理代码冲突、查看代码历史、回退代码版本等。

## 分布式版本控制系统（DVCS）

分布式 VCS 和中央式 VCS 的区别在于：分布式 VCS 除了有中央仓库之外，还有本地仓库，团队中的每个人的机器上都有一个本地仓库，这个仓库中保存着版本的所有历史，每个人都可以在自己的机器的本地仓库中提交代码、查看历史而无需联网与中央仓库交互，取而代之的，只需要和本地仓库交互。

中央式 VCS 的中央仓库有两个主要功能：`保存版本历史` /  `同步代码` 。而在分布式的 VCS 中，保存版本历史转移到了每个人的本地仓库，中央仓库只剩下同步代码这一个主要任务。当然中央仓库也会保存版本历史，不过这个历史只是作为团队同步代码的中转站。

## 快速上手

1. 在 github 上新建一个仓库： `git-practical-guide`

2. 在指定路径克隆该远程仓库到本地： `git clone https://github.com/yuzh233/git-practical-guide.git`

3. 查看日志： `git log` 

   ```bash
   $ git log
   commit 0a00230727018a14d481fe482e8e85f7b312e39c (HEAD -> master, origin/master, origin/HEAD)
   Author: yu_zh <yuzh233@163.com>
   Date:   Wed Oct 3 00:23:27 2018 +0800

       Initial commit
       
   -- github 默认创建了一次提交

   ```

4. 新建一个文件，自己创建一个提交：但先查看一下状态 `git status`

   ```bash
   $ git status
   On branch master
   Your branch is up to date with 'origin/master'.

   Untracked files:
     (use "git add <file>..." to include in what will be committed)

           README.md

   nothing added to commit but untracked files present (use "git add" to track)

   -- 当前分支是主分支，并且当前分支是最新的（相对于中央仓库）
   -- 有一个未追踪的文件 “README.md” ,可以使用 git add 追踪文件（提交）
   ```

5. 追踪文件： `git add README.md`，再次 `git status`。 添加全部文件到暂存区：`git add . / git add -A`

   ```bash
   $ git status
   On branch master
   Your branch is up to date with 'origin/master'.

   Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)

           new file:   README.md

   -- 当前分支是最新的，已暂存一个新文件：README.md，该文件由“未追踪”变成了“已暂存”，表明这个文件被改动的部分已进入“暂存区”
   ```

6. 提交文件：`git commit` 。进入 vim 填写提交的描述信息保存退出即可。也可以 `git commit -m "描述信息"`

   ```bash
   $ git commit

   *** Please tell me who you are.

   Run

     git config --global user.email "you@example.com"
     git config --global user.name "Your Name"

   to set your account's default identity.
   Omit --global to set the identity only in this repository.

   fatal: unable to auto-detect email address (got 'Administrator@YU-ZH.(none)')

   -- 首次提交需要指明身份（额外一点：github的贡献值是以邮箱地址作为提交的记录标识，如果 setting 里面没有添加该邮箱，那么贡献值会不存在。）

   Administrator@YU-ZH MINGW64 /d/IdeaProjects/git-practical-guide (master)
   $ git config --global user.email "yuzh233@gmail.com"

   Administrator@YU-ZH MINGW64 /d/IdeaProjects/git-practical-guide (master)
   $ git config --global user.name "yu.zh"

   Administrator@YU-ZH MINGW64 /d/IdeaProjects/git-practical-guide (master)
   $ git commit
   [master 967f40b] add README.md
    1 file changed, 76 insertions(+)
    create mode 100644 README.md

   ```

7. 再查看一次日志吧：`git log`

   ```bash
   $ git log
   commit 967f40b3fd29f102fc84f62ac9d20243db2a99b4 (HEAD -> master)
   Author: yu.zh <yuzh233@gmail.com>
   Date:   Wed Oct 3 21:50:03 2018 +0800

       add README.md

   commit 0a00230727018a14d481fe482e8e85f7b312e39c (origin/master, origin/HEAD)
   Author: yu_zh <yuzh233@163.com>
   Date:   Wed Oct 3 00:23:27 2018 +0800

       Initial commit

   -- 可以看到，一共有两次提交记录，最近的一次提交在最前面。
   ```

8. 当我们的文件有修改时，需要更新到本地仓库。先看一下状态是个好习惯： `git status`

   ```bash
   $ git status
   On branch master
   Your branch is ahead of 'origin/master' by 1 commit.
     (use "git push" to publish your local commits)

   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git checkout -- <file>..." to discard changes in working directory)

           modified:   README.md

   no changes added to commit (use "git add" and/or "git commit -a")

   -- 当前位于主分支，当前分支“领先于”远程仓库主分支一个提交
   -- 未提交的一个更改 README.md ，git 认识这个文件，但它不是一个新文件了，我们把它同步为最新的。
   ```

9. 依然是： `git add README.md`

10. 查看状态： `git status`

    ```bash
    $ git status
    On branch master
    Your branch is ahead of 'origin/master' by 1 commit.
      (use "git push" to publish your local commits)

    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

            modified:   README.md

    -- 一个文件被改动了
    ```

11. 提交： `git commit`

12. 此时查看下日志：`git log`

    ```bash
    $ git log
    commit 7ba78ab8cef21de0174884ef13fb6210f9552fe3 (HEAD -> master)
    Author: yu.zh <yuzh233@gmail.com>
    Date:   Wed Oct 3 22:10:13 2018 +0800

        update README.md

    commit 967f40b3fd29f102fc84f62ac9d20243db2a99b4
    Author: yu.zh <yuzh233@gmail.com>
    Date:   Wed Oct 3 21:50:03 2018 +0800

        add README.md

    commit 0a00230727018a14d481fe482e8e85f7b312e39c (origin/master, origin/HEAD)
    Author: yu_zh <yuzh233@163.com>
    Date:   Wed Oct 3 00:23:27 2018 +0800

        Initial commit

    ```

13. 此时本地仓库已经领先于中央仓库，把本地仓库推送到中央仓库：`git push` / `git push origin master`

**小结**

- git clone url

- git log

- git status

- git add 文件 / git add . / git add -A

- git commit / git commit -m "info"

- git push / git push origin master

## 本地初始化仓库并推送到 GitHub
1. 某个项目文件夹执行 `git init`

2. `git log / git add / git commit / git status` 啥的都来一遍

3. 在 github 创建与本地同名的仓库，如果本地有 README.md 远程就不要勾选创建这个文件，否则会冲突。

4. 关联远程库: `git remote add origin git@github.com:yuzh233/demo.git ` or `git remote add origin https://github.com/yuzh233/demo.git`

5. 推送到远程库：`git push -u origin master` ，需要先关联 GitHub 与本机的 SSH Key.

## 关联 GitHub 与本机的 SSH Key 
1. 先确保已经在 git 中保存了全局用户名和邮箱

    git config --global user.name "yourname"
    
    git config --global user.email“your@email.com"

2. 删除 C:\Users\Administrator\.ssh\known_hosts 文件

3. bash 输入 `ssh-keygen -t rsa -C "yuzh233@gmail.com"`，出现提示信息，一路回车，然后系统会自动在.ssh文件夹下生成两个文件，id_rsa 和 id_rsa.pub。

4. 打开 id_rsa.pub 将全部内容复制到 Github->setting->SSH and GPG keys->New SSH key->key

5. `ssh -T git@github.com`

## git pull

pull 指令用于从中央仓库更新代码到本地仓库。当多人协作开发时，同事 A 先于同事 B push 自己的本地仓库代码到中央仓库后，同事 B 再 push 自己的本地仓库时会报错，原因是远程仓库含有本地没有的 commit 而导致 push 失败。此时同事 B 需要使用 `git pull` 将同事 A push 到远程仓库的内容更新下来，这样同事 B 的本地仓库就含有远程仓库的commit了，然后同事 B 才能执行自己的 push。

一种场景：同事A　commit了一条记录并 push 到远程仓库。同事B 也在自己的本地仓库中 commit 了一条记录后试图 push。此时 git 会发现远程仓库含有本地未有的提交 push 失败。于是同事B 执行 `git pull` ，**此时的 pull 并不会和往常一样结束，而是会进入一个 vim 的信息提示界面，需要输入提交信息（git默认填写了信息），原因是git不仅发现远程仓库含有本地仓库没有的提交，本地仓库也含有远程仓库没有的提交，git会将远程仓库的 commit 和本地仓库的 commit 合并（git mage）产生一条新的提交并添加默认描述信息。** 退出保存信息提示界面之后，pull就完成了，然后执行 push，此时本地仓库含有远程仓库所有的提交不会失败。

## HEAD / master / branch 理解

指向 commit 的快捷方式：**引用**

![git log](git_img/image_1.png)

括号里的 `HEAD -> master, origin/master, origin/HEAD` ，都是指向这个 commit 的引用。commit 后面一大串的字符是当前提交的唯一标识符（SHA-1 校验和），提供引用机制是为了简化标识符，方便记忆。

**HEAD** 指向当前最新的 commit ，当前 commit 在哪里，HEAD 就在哪里，这是一个永远指向当前 commit 的引用。

HEAD 除了可以指向 commit，还可以指向一个 **branch**，当它指向某个 branch 的时候，会通过这个 branch 来间接地指向某个 commit；另外，当 HEAD 在提交时自动向前移动的时候，它会像一个拖钩一样带着它所指向的 branch 一起移动。

我们创建一个 commit 之后查看 log：

![](git_img/image_2.png)

最新的 commit 被创建后，HEAD 和 master 这两个引用都指向了它，而在上面第一张图中的后两个引用 origin/master 和 origin/HEAD 则依然停留在原先的位置。

**branch** 可以理解为从初始 commit 到 branch 所指向的 commit 之间的所有 commit 集合的一个串。

- 所有的 branch 之间都是平等的

- branch 包含了从初始 commit 到它的所有路径，而不是一条路径。并且，这些路径之间也是彼此平等的。

**master** 是一个特殊的 branch ,是git默认的分支（主分支）。新创建一个 repository 的第一个 commit 时，会把 master 指向它，并把 HEAD 指向 master。

## branch xx / checkout branch xx / checkout -d xx / branch -d xx 

创建一个分支：`git branch feature1`

切换到这个分支：`git checkout feature1`，此时 HEAD 指向了 feature1 这和分支了。

    创建一个分支并切换过去：git checkout -b feature1

我们在 feature1 分支中创建一个提交：添加 feature1.txt。此时 HEAD
指向了 feature1，feature1 指向当前提交。

又切换回 master 分支：`git checkout master`，在 master 创建一个提交：添加 master.txt，此时出现了分叉（两个分支有不同的提交）。

删除刚刚创建的分支：`git branch -d feature1`

- HEAD 指向的 branch 不能删除。如果要删除 HEAD 指向的 branch，需要先用 checkout 把 HEAD 指向其他地方。

- branch 只是一个引用，删除引用并不会删除该引用路径上的所有 commit 集合（不过一个 commit 不在任何一个 branch 路径上，就是个野生 commit 了，会被 git 垃圾回收掉）

- 没有被合并到 master 过的 branch 在删除时会失败。强制删除将 `-d` 改为 `-D`

