# Git 实用指南

## 一、版本控制系统（VCS）

**版本控制** / **主动提交** / **中央仓库** 构成了一个最核心的版本控制系统。

### 1.1 版本控制：最基本的功能

版本控制系统最基本的功能是版本控制。版本控制，简单的理解就是在文件中的修改历程中保存修改历史，我们可以方便的撤销之前对文件的修改。

在普通文本编辑器中，我们可以使用 Undo 操作回退到上一次的操作；在程序编码，我们可以通过 VCS 回退到指定的一次操作，而不仅仅是上一次操作。

### 1.2 主动提交机制：VCS 与普通文本编辑器的区别

使用普通文本编辑器的时候，一次保存就是一次改动，对版本的 `控制` 仅仅是回退到上一次操作。而正常情况下，我们的程序代码修改的生命周期十分长，一次代码的修改，在几天后、几个月后、甚至几年后都可能被翻出来。此时像普通编辑器的“自动保存提交”的功能在对历史代码审查、回退中会变得非常繁琐和无章可循。所以和普通文本编辑器的“撤销”功能不同，VCS 保存修改历史，使用 `主动提交改动` 的机制。

所谓 `主动提交改动` ，是指每次代码的修改和保存不会自动提交，需要手动提交（commit）到仓库，VCS 会把这次提交记录到版本历史中，当往后需要回退到这个版本，可以在 VCS 的历史提交纪录中找到这条记录。

### 1.3 中央仓库：多人合作的同步需求

中央仓库作为代码的存储中心，所有人的改动都上传到这里，所有人都可以看到并下载别人上传的改动。

版本控制 / 主动提交 / 中央仓库 这三个要素，共同构成了版本控制系统 VCS 的核心：开发团队中的每个人向中央仓库中主动提交自己的改动和同步别人的改动，并在需要的时候查看和操作历史的版本，这就是版本控制系统。

### 1.4 中央式版本控制系统

最基本的模型是：在一台服务器中初始化一个中央仓库，每个人从中央仓库下载初始版本开始并行开发，提交各自的代码到中央仓库并更新其他人的代码同步到自己的机器上。

团队中的每个人需要做的就是：1. 第一次加入团队，从中央仓库取代码到本地； 2. 写好的新功能提交到中央仓库； 3. 同事有新的代码提交，及时同步到本地。实际开发中当然还会经常需要处理代码冲突、查看代码历史、回退代码版本等。

## 二、分布式版本控制系统（DVCS）

分布式 VCS 和中央式 VCS 的区别在于：分布式 VCS 除了有中央仓库之外，还有本地仓库，团队中的每个人的机器上都有一个本地仓库，这个仓库中保存着版本的所有历史，每个人都可以在自己的机器的本地仓库中提交代码、查看历史而无需联网与中央仓库交互，取而代之的，只需要和本地仓库交互。

中央式 VCS 的中央仓库有两个主要功能：`保存版本历史` /  `同步代码` 。而在分布式的 VCS 中，保存版本历史转移到了每个人的本地仓库，中央仓库只剩下同步代码这一个主要任务。当然中央仓库也会保存版本历史，不过这个历史只是作为团队同步代码的中转站。

## 三、快速上手

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

5. 追踪文件： `git add README.md`，再次 `git status`

   ```bash
   $ git status
   On branch master
   Your branch is up to date with 'origin/master'.

   Changes to be committed:
     (use "git reset HEAD <file>..." to unstage)

           new file:   README.md

   -- 当前分支是最新的，已暂存一个新文件：README.md，该文件由“未追踪”变成了“已暂存”，表明这个文件被改动的部分已进入“暂存区”
   ```

6. 提交文件：`git commit` 。可以在后面以双引号包裹要添加提交的描述信息，如果不填将进入 vim 填写后保存退出即可。

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

