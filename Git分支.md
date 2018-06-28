# 基本原理
Git默认的一个主分支是master
HEAD只想master，master指向提交

![image](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849087937492135fbf4bbd24dfcbc18349a8a59d36d000/0)

你的每一次提交，master分支都会往前移动，随着提交的增多，分支会越来越长，
创建新的分支是，Git会新建一个指针只想新建时master只想的位置，然后HEAD指向新建制镇，表示当前分支在新建的上面（新建分支dev）![image](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908811773187a597e2d844eefb11f5cf5d56135ca000/0)
此后我们队工作区的修改和提交就是在对新建分支进行操作，同事，master还指向它原来的位置
![image](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849088235627813efe7649b4f008900e5365bb72323000/0)
当我们在分支上的工作结束了，想要将主分支和新建分支合并起来的话，只需要将master指向新建分支的当前提交就OK了，这就是两个分支的合并
![image](https://cdn.liaoxuefeng.com/cdn/files/attachments/00138490883510324231a837e5d4aee844d3e4692ba50f5000/0)

甚至我们也可以删除分支，即把指针删除，

# 命令：
- 创建分支dev并切换到该分支
```
git checkout -b dev 
```
等价于

```
git branch dev
git checkout dev
```
- 查看当前分支

```
git branch
```
*标注的是你的当前分支


- 切换分支

```
git checkout master
```
- 合并分支(合并指定分支到当前分支)

```
git merge dev
```
这种方式是“快进模式”，直接将master指向dev的当前提交，合并速度极快

- 删除分支

```
git branch -d dev
```
# 分支冲突
但是当我们创建了一个分支并在上面开始工作时，不可避免的需要在master上进行一些修改，这时，master指向了更前面的修改，而新建分支也可能越走越远，于是，在我们进行分支的合并时就出现了分支冲突的情况![image](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909115478645b93e2b5ae4dc78da049a0d1704a41000/0)
此时，我们合并两个分支时会出现冲突
通过git status可以查看冲突文件
同事产看冲突文件会发现，Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容
此时需要我们手动去修改一下冲突的内容，再去提交
此时两个分支如下图所示
![image](https://cdn.liaoxuefeng.com/cdn/files/attachments/00138490913052149c4b2cd9702422aa387ac024943921b000/0)

通过以下命令查看分支合并情况

```
git log --graph --pretty=oneline --abbrev-commit
```

前面我们介绍的都是“快进模式”来合并分支，这种模式下，删除掉分支后，会丢失分支信息
如果禁用了 Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息

```
git merge --no-ff -m "merge with no-ff" dev
```
这样的合并方式新建了一个commit，因此我们可以加-m参数添加描述
![image](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909222841acf964ec9e6a4629a35a7a30588281bb000/0)

## 分支策略
在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；

你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

所以，团队合作的分支看起来就像这样：
![image](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0)

#### 可能用到的
把当前工作现场“储藏”起来，等以后恢复现场后继续工作
```
git stash
```
查看保存的工作现场
```
git stash list
stash@{0}: WIP on dev: f52c633 add merge
```
工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；

另一种方式是用git stash pop，恢复的同时把stash内容也删了：

```
$ git stash pop
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   readme.txt

Dropped refs/stash@{0} (5d677e2ee266f39ea296182fb2354265b91b3b2a)
```
恢复到指定的stash

```
git stash apply stash@{0}
```


小结
修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。

- 如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

# 多人协作
当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。

要查看远程库的信息，用git remote：

```
git remote
```
查看更多关系可以加上参数-v

```
git remote -v
```
会显示你可以抓取和推送的origin的地址，如果没有推送权限，就看不到push的地址

#### 推送分支
推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

```
git push origin master
```
如果想要推送到远程仓库其他分支可以将master改成该分支的名称
多人协作的工作模式通常是这样：

首先，可以试图用git push origin <branch-name>推送自己的修改；

如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

如果合并有冲突，则解决冲突，并在本地提交；

没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！

如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。
小结
查看远程库信息，使用git remote -v；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。