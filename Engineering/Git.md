# Git

### 核心概念

- 快照（snapshot）副本
- 仓库（Repository）保存文件快照的数据库
- 工作区（working directory | workspace）
- 暂存区（index | stage）
- commit
    + git 采用全量记录的策略来保存每个版本
    + 每个 commit 就是一个版本，都代表整个项目所处的一个状态
    + 每个 commit 都包含上一个 commit 的引用，由此形成一条链

> 工作区、暂存区、commit 都是整个项目的完整副本，可以抽象成三颗独立的树

> staged changes 是暂存区与 HEAD 两颗树 diff 的结果

> unstaged changes 是工作区与暂存区这两颗树 diff 的结果


- HEAD：对当前分支最后一次 commit 的引用
    + 查看 HEAD 的 commit 信息：`git cat-file -p HEAD`
    + 查看 HEAD 的文件快照：`git ls-tree -r HEAD`
    + HEAD 的父节点：`HEAD~`
- 分支：指向某个 commit 的引用

> 后缀 ~ 表示父节点，~n 表示第 n 个父节点，[参考](https://stackoverflow.com/questions/2221658/whats-the-difference-between-head-and-head-in-git)

```js
// git 默认创建 master 引用，指向某个 commit
// commit-a <-- master <-- HEAD
{
    master: 'commit-a'
}

// 创建分支，就是新增引用
// commit-a <-- master
//          <-- branch <-- HEAD
{
    master: 'commit-a',
    branch: 'commit-a'
}

// 当前处于哪个分支，是由 HEAD 引用决定的，比如切回 master
// commit-a <-- master <-- HEAD
//          <-- branch
```


### 文件状态的流转

用 `git status -s（--short）`可以看到由符号表示的状态。符号有两位置，左边的是暂存区和上一次 commit 比较得到的状态，右边是工作区和暂存区比较得到的状态。状态只有四种：未知、新增、修改、没变化

- 新增文件 F（untracked） `??`
- `git add F`
- 已暂存（staged new file） `A `
- `修改 F`
- 已暂存和已修改（staged new file & modified）`AM`
- `git commit`
- 已修改（modified & to be staged for commit）` M`
- `git add F`
- 已修改（modified & to be committed）`M `
- `git commit`
- 已保存 👌
- `修改 F`
- 已修改（modified & to be staged for commit）` M`
- `git add F`
- 已修改（modified & to be committed）`M `
- `修改 F`
- 已修改（modified & to be committed & to be staged for commit）`MM`


### 记录变更

记录变更有两个命令：add 和 commit

为什么要设计暂存区？暂存区本质上是准备添加到下一个 commit 的候选内容，暂存区允许用户更灵活的控制将哪些内容作为下一个 commit。比如当前修改了 10 个文件，但只挑选其中 3 个作为一次 commit 提交，如果中途反悔，还可以撤出某个修改，再将剩下的作为 commit 提交。

补漏。如果还想向上一个 commit 继续添加一些东西，用 `git commit --amend`

合写。添加暂存并提交：`git commit -am"xxx"`


### 查看历史变更记录

- `git log`
- `git log -p` 查看历史 commit 记录以及每次的变更
- `git log -p -2` 查看最近两次 commit 及其变更
- `git log --stat`
- `git log --graph`

### 撤销更改/删除记录

reset 本质是在操控 HEAD、暂存区和工作区这三棵树

语法：`git reset [commit id] [file] --[mode]`

commit 默认为 HEAD，file 默认为空，mode 默认为 mixed

`git reset` 三种模式

- `--soft` 仅仅移动/变更 HEAD 指向
- `--mixed` （默认）移动 HEAD 指向，并将 HEAD 复制到暂存区，相当于重置整个暂存区
- `--hard` 移动 HEAD 指向，并将 HEAD 复制到暂存区和工作区，相当于重置整个暂存区和工作区

> 如果没有指定 file，则整体复制；若指定 file，则仅复制文件

**实例**

- 取消暂存：`git reset` or `git reset HEAD` or `git reset master`（假设当前在 master 分支）
- 取消某个已暂存文件：`git reset file`
- 放弃一部分 commit，但不放弃暂存和工作区的变更：`git reset <id> --soft`
- 彻底放弃所有变更，回到某个 commit：`git reset <id> --hard`
- 撤销当前的 merge：`git reset HEAD~ --hard`

根据 reset 的工作原理，显然无法用 reset 取消当前尚未暂存的更改，解决办法是：git checkout

- 取消某个文件的尚未暂存的变更：`git checkout <file>`，含义是：从暂存区的树中复制 file 并覆盖当前的 file
- 重置整个工作区：`git checkout .`

如何删除任意某个 commit 呢？

如何删除文件的版本追踪记录？

- 如果文件没有新的暂存更改（暂存区和 HEAD 记录一致），则用 `git rm <file>` 将文件从**暂存区**移除，同时会删除文件
- 如果已经有新的更改提交到暂存区，为了彻底移除，需要 `git rm <file> -f`
- 如果不想删除文件，仅仅是删除 git 记录：`git rm --cached <file>`
- 如果是删除文件夹，需要明确指定 `-r`（recursively）
- 还可以用 glob 指定删除目标

**实例**

- 如果把 node_modules 给加到了版本追踪，忘了用 gitignore 怎么办？`git rm --cached -r node_modules`

- 不小心把一个特别大的文件加到了版本管理，导致 .git 特别大，想删除该文件的全部历史记录怎么办？


### 比较差异


### 分支


### 与远程交互

在企业里，git 仓库结构一般如下（远程仓库只有一个）

```
remote
  |-- local
  |-- local
  |-- local
  |-- ...
```

在 github 上，git 仓库结构大概如下（对每个本地仓库而言，远程仓库有多个）

```
origin -- local
  |
remote -- local
  |
remote -- local
  |
remote -- local
  | ...
```

查看所有的远程仓库：`git remote -v`

查看某个远程仓库：`git romote show <remote-name>`

添加远程仓库：`git remote add <shortname> <url>`

拉取与推送

- `git clone <url>`
- `git fetch [remote-name] [branch]`
- `git fetch [remote-name]`
- `git pull [remote-name]`：建立追踪关系后，可以省略远程分支名
- `git pull <remote-name> <远程分支A>:<本地分支B>`：拉取远程分支 A 与本地分支 B 合并
- `git pull <remote-name> <远程分支A>`：拉取远程分支 A 与本地当前分支合并
- `git push [remote-name] [branchname]`

> 使用 clone 命令克隆一个仓库时，命令会自动将其添加为远程仓库并默认以 “origin” 为简写

**实例**

- 如何删除远程的 commit：在本地删除，然后 `git push origin +master` or `git push origin master -f`
- 删除远程分支：`git push origin --delete [branch]`
- 删除远程文件：``
