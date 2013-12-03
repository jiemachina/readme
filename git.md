# 问题或目的
[参考材料](http://git-scm.com/book/zh/)<br>
搜集自己工作中常用的git命令

## git的基本配置
```
git config --global user.name "majie"
git config --global user.email "your@email.com"

```
<p>
更多配置信息
</p>

```
user.name=yourname
user.email=your@email.com
color.ui=true
color.status=auto
color.diff=auto
color.branch=auto
color.interactive=auto
core.editor=mvim
core.quotepath=false

```

## 生成SSH Key
`ssh-keygen -t rsa
`

## 常用的Git

* git init
* git add .
* git commit -m 'you commit recommend'
* git log
* git push origin master
<p>
以上几点是常用的流程
</p>

* git checkout .
* git reset HEAD `file`: 取消跟踪
* git reset --hard HEAD~1: 硬取消最后一次提交（一次白费，不建议使用）
* git reset --soft HEAD~1: 软取消最后一次commit, commit的后悔药
* git blame `your file`: 查看最后是由谁修改的
* git show `hash code`: 查看最后一个commit的代码修改
* git reflog: 查看最近操作的
* git stash: 缓存当前编辑

