#GIT学习笔记

[toc]


---

###基本流程
#####创建仓库
- 在*GITHUB*创建仓库
用*git clone*将仓库克隆到本地
- 在本地创建仓库
在*GITHUB*创建同名仓库
用*git remote add*添加远程仓库
用*git push*将本地仓库推送至*GITUHUB*

---

###GIT指令

#####git init
初始化仓库

#####git status
查看当前仓库的状态信息

#####git add
向暂存区中添加文件
```
$ git add README.md
```

#####git commit
保存仓库的历史记录
- 默认打开编辑器：
```
$ git commit
```
不使用-m参数，自动打开编辑器，在编辑器中输入提交信息，关闭编辑器则确认更改
- 记述一行提交信息
```
$ git commit -m "First commit"
```
*不会打开编辑器*
- 修改上一条提交信息
```
$ git commit --amend
```
- 用**git commit -am**一次完成git add和git commit操作
```
$ git commit -am "Add feature-C"
```

#####git log
查看文件提交日志
- 默认查看详细信息
```
$ git log
```
- 以图表形式查看日志
```
$ git log --graph
```
- 只显示提交信息的第一行
```
$ git log --pretty=short
```
- 只显示指定目录、文件的日志
```
$ git log README.md
```
- 显示文件的改动
```
$ git log -p
```
只查看特定文件的改动：
```
$ git log -p README.md
```

#####git diff
查看更改前后的差别
- 查看工作树与最新提交的差别
```
$ git diff HEAD
```

#####git branch
显示分支一览表
- 创建分支
```
$ git branch test
```
- 删除分支
```
$ git branch -d test
```

#####git checkout
切换分支
```
$ git branch feature-A
```
- 创建并切换分支
```
$ git checkout -b feature-A
```
*也可以使用如下命令*
```
$ git branch feature-A
$ git checkout feature-A
```
- 切换到上一个分支
```
$ git checkout -
```
- 获取远程的分支
```
$ git checkout -b feature-D origin/feature-D
```

#####git merge
合并分支
```
$ git merge --no-ff feature-A
```

#####git reset
回溯历史版本
```
$ git reset --hard HASH_VALUE
```
*HASH_VALUE为目标时间点的哈希值*

#####git reflog
查看当前仓库的操作日志

#####git rebase
压缩历史
```
$ git rebase -i HEAD~2
```
选定当前分支中包含HEAD在内的两个最新历史记录为对象并在编辑器中打开

#####git remote add
添加远程仓库
```
$ git remote add origin git@github.com:github-book/git-tutorial.git
```
*自动将远程仓库的名称设置为origin（标识符）*

#####git push
推送至远程仓库
```
$ git push -u origin master
$ git push -u origin feature-D
```

#####git clone
从GITHUB克隆仓库到本地
```
$ git clone git@github.com:github-book/git-tutorial.git
```

#####git pull
获取最新的远程仓库分支
```
$ git pull origin feature-D
```
*将本地的feature-D分支更新到远程仓库的最新状态*

#####git fetch
从远程获取代码库
```
$ git remote add upstream git://github.com/octocat/Spoon-Knife.git
$ git fetch upstream
$ git merge upstream/master
```
*用远程代码库更新本地仓库*

