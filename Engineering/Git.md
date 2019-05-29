# Git 典型场景及用法

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


### 查看历史变更记录


### 撤销记录


### 比较差异


### 分支


### 与远程交互
