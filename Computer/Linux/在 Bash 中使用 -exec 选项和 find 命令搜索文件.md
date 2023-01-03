​

 转自：[https://www.delftstack.com/zh/howto/linux/linux-find-exec/](https://www.delftstack.com/zh/howto/linux/linux-find-exec/ "https://www.delftstack.com/zh/howto/linux/linux-find-exec/")

我们可以使用带有 `-exec` 选项的 `find` 命令来查找包含我们要搜索的文本的文件。

主要概念是使用 `find` 命令获取工作目录中的每个文件，并执行 `grep` 命令查找每个文件中的文本。

例子：

```
# !/bin/bash
find . -exec grep linux {} \;
```

![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "点击并拖拽以移动")

以下命令将返回找到指定 `text` 的行。

输出：

```
find . -exec grep linux {} \;
find . -exec grep linux {} +
title = "Unzip .gz file in linux"
description = "How to unzip a .gz file in linux"
```

![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "点击并拖拽以移动")

防止 shell 解释 `;` 分隔符，我们在它之前使用`\`。使用这种策略，我们只得到检测到文本的行。

我们可以通过替换分隔符 `;` 来获取行以及找到它的文件名带有`+`。

```
# !/bin/bash
find . -exec grep linux {} +
```

![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "点击并拖拽以移动")

输出：

```
./bash.sh:find . -exec grep linux {} \;
./bash.sh:find . -exec grep linux {} +
./unzip_gz_linux.txt:title = "Unzip .gz file in linux"
./unzip_gz_linux.txt:description = "How to unzip a .gz file in linux"
```

![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "点击并拖拽以移动")

## `find` 处理表达式结果的方式由分隔符决定：

如果我们使用分号 `;` `-exec` 命令将独立重复每个结果。

如果我们**使用`+` 符号，所有表达式的结果将被连接起来并提供给 `-exec` 命令，只运行一次**。出于性能原因，我们更喜欢使用 `+` 分隔符。

​