Git的标签可以唯一标记一个版本，并且可以通过标签渠道该版本的内容
# 创建标签
- 给当前分支打上标签
```
git tag v1.0
```

查看所有标签

```
git tag
```
标签默认打在最新提交的commit上
但是也可以为指定commit id的commit打标签
首先查看历史提交的commit id

```
git log --pretty=oneline --abbrev-commit
```

```
git tag v1.2 <commit id>
```
查看某个标签详细信息

```
git show v1.0
```
还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：


```
git tag -a v0.1 -m "version 0.1 released" 1094adb
```

- 删除标签(本地)

```
git tag -d v1.0
```
- 因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

如果要推送某个标签到远程，使用命令git push origin <tagname>：

```
git push origin v1.0
```
- 一次性推送全部的本地标签到远程

```
git push origin --tags
```
- 删除标签（远程）

```
git tag -d v1.0
git push origin :refs/tags/v1.0
```

