# 彻底删除GitHub仓库的某个文件或文件夹及其历史记录

## 缘起
最近写blog的代码，误把带有自己邮箱的SMTP的后台接口文件一起push到远程仓库了。

如果直接删除并提交该文件的话，依然能从git log 中查看到该文件，所以需要将该文件删除的同时，删除所有commit的记录才可以。

github官方参考传送门：<https://help.github.com/articles/remove-sensitive-data/>

特此记录删除对应**文件**（**文件夹**）的过程。

## 步骤如下：

### 首先，重写git log，删除文件和对应的log:

~~~ cpp
git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch FILE_PATH' --prune-empty --tag-name-filter cat -- --all
~~~
> 注：‘FILE_PATH’是该文件所在的路径(本地的路径也会被删除)

### 然后，强制推到远端所有分支:
~~~ cpp
git push origin master --force
~~~

### 最后，这个文件还在你的本地仓库里，还需要将它完全抹除:
~~~ cpp
rm -rf .git/refs/original/
git reflog expire --expire=now --all
git gc --prune=now
git gc --aggressive --prune=now
~~~
