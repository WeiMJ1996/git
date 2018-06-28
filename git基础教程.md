# 基本命令
```
git init
```
```
git add
```
 添加到暂存区仓库（可以多次添加，然后一起提交）


```
git commit -m "..."
```
 提交到仓库分支

```
git status
```
 查看状态
```
git log
```
查看操作日志

提交后，用git diff HEAD --

<file>命令可以查看工作区和版本库里面最新版本的区别
```
git diff HEAD --<file>
```

```
git reset --hard HEAD^
```
 回退到上一版本

```
git reset --hard jfdj(版本号)
```
 回退到指定版本号

```
git reflog
```
 记录每一次命令

# 工作区和版本库

目录是工作区 .git是版本库

工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。


```
git checkout -- file
```
丢弃工作区的修改


```
git reset HEAD <file>
```

可以把暂存区的修改撤销掉（unstage），重新放回工作区

```
git rm <file> 
git commit
```
删除版本库里的文件

```
git remote add origin git@github.com:michaelliao/learngit.git
```

把本地库街远程库关联起来

```
git push -u origin master
```
提交到远程仓库（已经关联过的，这个origin就是那个远程仓库的名字 可以绑定多个 选择提交到哪里）

但是经常提交的时候，别人也会提交一些内容到远程仓库，造成了我们贝蒂仓库和远程仓库的不一致

这样造成的问题就是 在我们提交到远程仓库的时候会出现error：failed to push some refs to这样的错误

因此我们需要先把远程库同步到本地库来


```
git pull --rebase origin master
```
这条指令的意思就是把远程库中的更新合并到本地库，--rebase的作用是取消本地库刚刚的commit，并把他们接到更新后的版本库中


```
git pull –rebase origin master
```
意为先取消commit记录，并且把它们临时 保存为补丁(patch)(这些补丁放到”.git/rebase”目录中)，之后同步远程库到本地，最后合并补丁到本地库之中。

之后就可以提交内容到本地库中了
