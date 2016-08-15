## 安装git

### windows平台

[下载](https://git-for-windows.github.io/)

按默认设置安装。

pull远程仓库——`git pull origin master`

push到远程——`git push origin master`

从远程仓库克隆：

```
git clone git@github.com:michaelliao/bootstrap.git
```

## 创建版本库

```git
mkdir learngit
cd learngit
git init
```



## git的基本功能

初始化仓库，`git init`

添加文件到仓库，分两步：

- `git add readme.md`
- `git commit -m "add a file"`

(-m 后面跟着说明)

`git status`查看状态

`git diff readme.txt` 可查看之前修改的地方

`git log --pretty=oneline`

### 版本回退

`HEAD`表示当前版本，`HEAD^`指上一个，`HEAD^^`是上上个，……多了不好写，所以写成`HEAD~100`

**回退到上一个版本**——`git reset --hard HEAD^`

版本回退后后悔了怎么办？

`git reflog`查看每一次命令，找到版本号

`git reset --hard commit_id`

### 管理修改

```
git diff HEAD -- readme.txt 
```

可以查看工作区和版本库里面最新版本的区别。

### 撤销修改

命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在**工作区**的修改全部撤销，这里有两种情况：

Git同样告诉我们，用命令`git reset HEAD file`可以把暂存区的修改撤销掉（unstage），重新放回工作区：

`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。

- 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。
- 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD file`，就回到了场景1，第二步按场景1操作。
- 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013744142037508cf42e51debf49668810645e02887691000)一节，不过前提是没有推送到远程库。

### 删除文件

`rm test.txt`

两个选择：

- 一是确实要从版本库中删除该文件，那就用命令`git rm text.txt`删掉，并且`git commit`：
- 另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

```
$ git checkout -- test.txt
```

## git的分支管理

创建`dev`分支，并切换到上面：

`git checkout -b dev`

其实相当于这两条命令：

- `git branch dev`
- `git checkout dev`

查看当前分支：`git branch`

操作，提交。

dev分支工作完后，切换回master分支：

`git checkout master`

将dev分支上的工作成果合并到master上

`git merge dev`

**Fast-forward** 合并是快进模式，速度非常快。（不是每次都能用这种模式）

合并完成后，可以放心删除`dev`了

`git branch -d dev`

git 鼓励使用分支完成某个任务，合并后再删除。

（先回到主分支，再删除。）

> Git鼓励大量使用分支：

> 查看分支：`git branch <name>`

> 创建分支：`git branch <name> `

> 切换分支：`git checkout <name> `

> 创建+切换分支：`git checkout -b <name> `

> 合并某分支到当前分支：`git merge <name> `

> 删除分支：`git branch -d <name> ` 

![](http://www.liaoxuefeng.com/files/attachments/001384909115478645b93e2b5ae4dc78da049a0d1704a41000/0)

### 解决冲突

两个分支都有新的提交时，怎么办？

（无法执行快速合并，即无法`git merge <name>`）

[冲突解决办法](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840202368c74be33fbd884e71b570f2cc3c0d1dcf000)

文本中会用》》和《《标记出来，要**手动修改**

- 当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

`git log --graph` 看分支合并图。

git默认使用Fast Forward模式，这种模式下，删除分支后，会丢掉分支信息。

`--no-ff`

```
git merge --no-ff -m "merge with no-ff" dev
```

即禁用fast Forward模式，这样就成了：



![git-no-ff-mode](http://www.liaoxuefeng.com/files/attachments/001384909222841acf964ec9e6a4629a35a7a30588281bb000/0)

于是有了dev分支，有机会可以恢复了。

因此这种模式更常用。

### BUG分支

你遇到个bug需要修复，而手头的工作还没完成，怎么办？

`stash`功能

输入`git stash`

暂时将手头工作存起来，腾出一个干净的工作空间。

按以前的方法，创建和切换分支，操作，add，commit，删除分支。

然后找回被git stash存放起来的内容。

两种恢复方法：

- 一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；
- 另一种方式是用`git stash pop`，恢复的同时把stash内容也删了（常用）

用`git stash list`确认

### Feature分支

没有合并时，新建的分支无法删除。

这时需要**强行删除**

`git branch -D feature`

(大写的D)

### 多人协作

`git remote`

`git remote -v`

`git push origin dev` 也是可以的

- 在本地创建和远程分支对应的分支，使用`git checkout-b branch-name origin/branch-name `

如果`git pull`失败，要用

`git branch --set-upstream dev origin/branchname`建立联系

## 创建标签

`git tag v1.0`

如果忘了打标签：

```
git log --pretty=oneline --abbrev-commit
```

找到版本号：

`git tag v0.9 664526`

q可以退出git log形式

### 操作标签

`git tag -d v1.0 `删除标签

`git push origin v1.0`推送标签到远程

`git push origin --tags` 	一次性推送所有标签

删除远程标签较复杂：

- 先从本地删除，git tag -d v0.9
- 再从远程删除，git push origin :refs/tags/v0.9

## git配置

比如用`git st`表示`git status`

只要敲入这行命令：

> git config --global alias.st status

git config --global alias.co checkout

git config --global alias.ci commit

(用简写可以代替很多字符，大大提高效率)

