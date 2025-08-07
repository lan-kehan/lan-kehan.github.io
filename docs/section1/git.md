# Git使用

学习branching的小网站 [Learn Git Branching](https://learngitbranching.js.org/)

## 简单工作流

初始化仓库，并且添加远程仓库

```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin <repository-url>
git push -u origin main
```

工作区修改完代码以后，跑通测试，查看当前状态

```bash
git status
```

明确添加的文件，并且提交

```bash
git add .
git commit -m "Fix bug"
```

查看当前日志

```bash
git log --oneline --graph --all
```

明确当前所在分支以及远程分支以后，推送到远程

```bash
git push -u origin main
```

## 分支操作

创建分支

```bash
git checkout -b new-feature
```

这样可以在新分支上commit

```bash
git add .
git commit -m "Add new feature"
```

合并分支，例如我要把new-feature分支合并到main分支
先切换到main分支

```bash
git checkout main
```

然后合并分支

```bash
git merge new-feature
```

或者rebase

```bash
git rebase new-feature
```

merge和rebase的区别在于，merge会创建一个新的合并提交，而rebase会将提交历史线性化，直接从分支节点剪掉当前分支接到要合并的分支上。

## 远程操作

查看远程仓库

```bash
git remote -v
```

先把远程的分支拉下来，指定分支名

```bash
git fetch origin main
```

看一下log

```bash
git log --oneline --graph --all
```

然后merge或者rebase（假设目前已经在main分支）

```bash
git merge origin/main
git rebase origin/main
```

fetch和合并可以用pull代替

```bash
git pull origin main
git pull --rebase origin main
```

然后更新远程的main分支

```bash
git push origin main
```

这样远程的main分支就有本地main分支的所有提交记录

## 其他操作

在分支之间切换

```bash
git checkout <branch-name>
```

重置当前分支（分支指针和HEAD一起回退）

```bash
git reset --soft HEAD~2
```

reset --hard会丢弃当前工作区和暂存区的所有修改，把工作区重置到指定的提交，之后的提交也会丢失。
如果用--soft选项，会保留工作区和暂存区的修改，只移动HEAD指针。
如果用--mixed选项（默认），会保留工作区修改，但清空暂存区。

还可以用checkout命令来查看某个历史提交，进入detached HEAD状态

```bash
git checkout <commit-id>
```

注意：这会让工作区内容变成指定提交的状态（改变HEAD指针），但不会改变分支指针。
要返回原分支，使用：

```bash
git checkout <branch-name>
```

临时储藏工作区修改

```bash
git stash push -m "描述信息"
```

查看储藏列表

```bash
git stash list
```

恢复储藏的修改

```bash
git stash pop    # 恢复最新储藏并删除
git stash apply  # 恢复最新储藏但不删除
```

删除储藏

```bash
git stash drop   # 删除最新储藏
git stash clear  # 清空所有储藏
```

这里stash同时保留工作区和暂存区的状态，恢复时会尽量保持原来的状态。

如果要把工作区某个文件恢复到某个提交的状态，可以用

```bash
git checkout <commit-id> -- <file>
```
注意此时HEAD不会改变。

这个时候如果要从上游提交开始再创建新的分支

```bash
git checkout -b new-branch <commit-id>
```

这相当于在上游提交的基础上创建了一个新的分支，HEAD指针会指向new-branch分支
