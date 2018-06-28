# 常用操作：
- 对比当前本地仓库分支和远程仓库分支差集

先更新本地的远程分支
然后进行对比
```
git fetch origin
git diff 本地分支 origin/xxxx
```
接下来对比2个版本的差异，可以用命令

```
git diff HEAD FETCH_HEAD
```
- 删除本地仓库内容 并推送到远程仓库

```
git rm -r --cached <file>
```

```
git commit -m "message"
```

```
git push
```

