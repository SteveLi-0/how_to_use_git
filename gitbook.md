# git使用

整理自廖雪峰 

https://www.liaoxuefeng.com/wiki/896043488029600/896827951938304

```
# 创建仓库
git init

# 把文件添加到仓库
git add readme.txt

# 文件提交到仓库
# git commit命令，-m后面输入的是本次提交的说明，可以输入任意内容
git commit -m "wrote a readme file"

# 仓库当前的状态
git status

# 查看修改内容
git diff readme.txt

# 查看git提交log
# head 表示当前版本
# head^ 表示上一个版本
# head~100 表示前100个版本
git log --pretty=oneline

# 回退版本
# 回退到上一个版本
git reset --hard HEAD^
# 恢复原来版本
git reset --hard commit id

# 命令记录
git reflog

```

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区 stage；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。



场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)一节，不过前提是没有推送到远程库。

```
# 撤销修改
# 命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
# 一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
# 一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
# 总之，就是让这个文件回到最近一次git commit或git add时的状态。
git checkout -- readme.txt

# 撤销进入stage的修改
git reset HEAD readme.txt
git restore --staged readme.txt

# 丢弃工作区的修改
git checkout -- readme.txt
```

将文件提交到版本库后，删除这个文件

```
# git add test.txt
# git commit -m "add test.txt"

# 工作区删除 rm test.txt

# 情况一
# 删除版本库中的文件
git rm test.txt
git commit -m "remove test.txt"

# 情况二
# 误删除，需要恢复工作区中的文件
# git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
# 只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。
git checkout -- test.txt

```



# 远程仓库

## ssh加密&添加远程仓库

https://www.liaoxuefeng.com/wiki/896043488029600/896954117292416

https://www.liaoxuefeng.com/wiki/896043488029600/898732864121440

要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

关联一个远程库时必须给远程库指定一个名字，`origin`是默认习惯命名；

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

```
#  push an existing repository from the command line
git remote add origin git@github.com:SteveLi-0/how_to_use_git.git
git branch -M main
git push -u origin main
```



## 