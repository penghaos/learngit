##创建版本库

创建一个版本库，选择一个合适的地方，创建一个空目录：

- $ mkdir learngit
- $ cd learngit
- $ pwd

第二步，通过`git init`命令把这个目录变成Git可以管理的仓库

- $ git init （初始化一个仓库）

---

第一步用`git add`把文件添加到仓库：

- $ git add readme.txt

第二步，用命令`git commit`告诉Git，把文件提交到仓库：

- $ git commit -m "wrote a readme file"

---

总共分两步：

1. git add<file>
2. git commit

##时光机穿梭

- git status （掌握仓库当前的状态）

- git diff readme.txt（查看做了什么修改）

##版本回退

- git log （显示最近的提交日志）

想好看一点可以加上`--pretty=oneline`

- git reset --hard HEAD^ (HEAD 指向的就是当前版本)
- git reset --hard 3628164 （版本号）

`git reflog` 查看命令历史，看所有的版本命令


---

工作区和暂存区，add是放入版本库里的暂存区，commit放入分支。

一个add就要搭配一个commit。


