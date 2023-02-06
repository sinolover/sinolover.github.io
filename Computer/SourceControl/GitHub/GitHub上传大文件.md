~~~bash
remote: error: GH001: Large files detected. You may want to try Git Large Filetorage - https://git-lfs.github.com.
remote: error: Trace: ~~
remote: error: See http://git.io/iEPt8g for more information.
remote: error: File ~~ is 273.28 MB;his exceeds GitHub's file size limit of 100.00 MB
To https://github.com/~~.git
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to 'https://github.com/~~.git'
~~~
错误原因：当单个文件超过100M的时候，push的时候会出现上面的Error。

解决方式：
~~~bash
# 1.安装Git命令行扩展。只需要设置一次Git LFS
$ git lfs install
# 2.选择您希望Git LFS管理的文件类型
$ git lfs track "*.psd"
# 确保跟踪.gitattributes
$ git add .gitattributes
# 3.Just commit and push to GitHub as you normally would.
$ git add file.psd
$ git commit -m "Add design file"
$ git push origin master
~~~