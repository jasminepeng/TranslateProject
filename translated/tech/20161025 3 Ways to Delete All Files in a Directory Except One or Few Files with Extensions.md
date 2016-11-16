# 删除目录下除特定扩展名文件外其它所有文件的三种方法

有时候，你可能会遇到这种情况，需要删除一个目录下的所有文件，除了一些指定类型（以特定扩展名结尾）的文件。

在这里，我们将展示如何通过 rm、 find 和 globignore 命令删除一个目录下指定文件后缀或者类型之外的文件。

在进一步深入之前，先简要介绍下 Linux 中的一个重要的概念 —— 文件名模式匹配，它可以帮助我们解决眼前的问题。

在 Linux 下，shell 模式是包含以下特殊字符（称为通配符或者元字符）的字符串，：

1.  `*` – 匹配 0 个或者多个字符
2.  `?` – 匹配任意单个字符
3.  `[seq]` – 匹配序列中的任意一个字符
4.  `[!seq]` – 匹配任意一个不在序列中的字符

我们将在这儿探索三种可能的办法，包括：

### 使用扩展模式匹配操作符删除文件

下面列出了不同的扩展模式匹配操作符,其中 patern-list 是用 `|` 分隔的一个或者多个文件名的列表：

1.  `*(pattern-list)` – 匹配 0 个或者多个指定模式
2.  `?(pattern-list)` – 匹配 0 个或者 1 个指定模式
3.  `+(pattern-list)` - 匹配一个或多个指定模式 
4.  `@(pattern-list)` – 匹配多个指定模式中的一个
5.  `!(pattern-list)` – 匹配除了指定模式之外的任何内容

要使用它们，先启用 extglob shell ，如下：

```
# shopt -s extglob

```

#### 1. 输入以下命令，删除一个目录下特定文件名之外的所有文件。

  ```
$ rm -v !("filename")

  ```
  [![Linux 下删除特定文件之外的所有文件](http://www.tecmint.com/wp-content/uploads/2016/10/DeleteAll-Files-Except-One-File-in-Linux.png)][9]

                            Linux 下删除特定文件之外的所有文件


#### 2. 删除 filename1 和 filename2 之外的所有文件：

  ```
$ rm -v !("filename1"|"filename2") 

  ```
  [![ Linux 下删除一些文件之外的所有文件](http://www.tecmint.com/wp-content/uploads/2016/10/Delete-All-Files-Except-Few-Files-in-Linux.png)][8]

                        Linux 下删除一些文件之外的所有文件 

#### 3. 下面的例子显示如何通过交互模式删除 `.zip` 之外的所有文件：

  ```
$ rm -i !(*.zip)

  ```
  [![ Linux 下删除 Zip 文件之外的所有文件](http://www.tecmint.com/wp-content/uploads/2016/10/Delete-All-Files-Except-Zip-Files-in-Linux.png)][7]

                     Linux 下删除 Zip 文件之外的所有文件

#### 4. 接下来，通过如下方式，可以删除一个目录下除了所有的`.zip` 和 `.odt` 文件外的所有文件，并且在删除的时候，显示正在进行的操作：

  ```
$ rm -v !(*.zip|*.odt)

  ```
  [![删除指定文件扩展名外的所有文件](http://www.tecmint.com/wp-content/uploads/2016/10/Delete-All-Files-Except-Certain-File-Extensions.png)][6]

                                        删除指定文件扩展名外的所有文件

执行完所有需要的命令后，使用如下方式关闭 extglob shell 。

  ```
$ shopt -u extglob

  ```

### 使用 Linux 下的 find 命令删除文件

在这种方法下，我们可以只[使用 find 命令][5]的适当的选项或者采用管道配合 xargs 命令，如下所示：

  ```
$ find /directory/ -type f -not -name 'PATTERN' -delete
$ find /directory/ -type f -not -name 'PATTERN' -print0 | xargs -0 -I {} rm {}
$ find /directory/ -type f -not -name 'PATTERN' -print0 | xargs -0 -I {} rm [options] {}

  ```

#### 5. 下面的命令将会删除当前目录下除了 `.gz` 之外的所有文件：

  ```
$ find . -type f -not -name '*.gz' -delete

  ```
  [![find 命令 —— 删除 .gz 之外的所有文件](http://www.tecmint.com/wp-content/uploads/2016/10/Remove-All-Files-Except-gz-Files.png)][4]

                      find 命令 —— 删除 .gz 之外的所有文件

#### 6. 使用管道和 xargs，你可以通过如下的方式修改上面的例子：

  ```
$ find . -type f -not -name '*gz' -print0 | xargs -0  -I {} rm -v {}

  ```
  [![使用 find 和 xargs 命令删除文件](http://www.tecmint.com/wp-content/uploads/2016/10/Remove-Files-Using-Find-and-Xargs-Command.png)][3]

                            使用 find 和 xargs 命令删除文件

#### 7. 再看一个例子，删除当前目录下除了 `.gz`、 `.odt` 和 `.jpg` 之外的所有文件：

  ```
$ find . -type f -not \(-name '*gz' -or -name '*odt' -or -name '*.jpg' \) -delete

  ```
  [![删除指定扩展名外的所有文件](http://www.tecmint.com/wp-content/uploads/2016/10/Remove-All-Files-Except-File-Extensions.png)][2]

                            删除指定扩展名外的所有文件

### 通过 bash 中的 GLOBIGNORE 变量删除文件

下面这最后的方法，只适用于 bash。 GLOBIGNORE 变量存储了一个模式列表（或文件名），该列表由冒号分隔，列表中的文件将被忽略。
 
使用此方法时，在要删除文件的目录下，像下面这样设置 GLOBIGNORE 变量：

```
$ cd test
$ GLOBIGNORE=*.odt:*.iso:*.txt

```

在这种情况下，除了 `.odt`、 `.iso` 和 `.txt` 之外的所有文件，都将从当前目录删除。

现在，运行如下的命令清空这个目录：

```
$ rm -v *

```

之后，关闭 GLOBIGNORE 变量:

```
$ unset GLOBIGNORE

```
[![使用 bash 变量 GLOBIGNORE 删除文件](http://www.tecmint.com/wp-content/uploads/2016/10/Delete-Files-Using-Bash-GlobIgnore.png)][1]

                                  使用 bash 变量 GLOBIGNORE 删除文件

就这些了！如果你可实现用其他命令行实现同样的效果，请分享给我们。

--------------------------------------------------------------------------------

via: http://www.tecmint.com/delete-all-files-in-directory-except-one-few-file-extensions/?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+tecmint+%28Tecmint%3A+Linux+Howto%27s+Guide%29

作者：[ Aaron Kili][a]

译者：[yangmingming](https://github.com/yangmingming)

校对：[jasminepeng](https://github.com/jasminepeng)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]: http://www.tecmint.com/author/aaronkili/
[1]:http://www.tecmint.com/wp-content/uploads/2016/10/Delete-Files-Using-Bash-GlobIgnore.png
[2]:http://www.tecmint.com/wp-content/uploads/2016/10/Remove-All-Files-Except-File-Extensions.png
[3]:http://www.tecmint.com/wp-content/uploads/2016/10/Remove-Files-Using-Find-and-Xargs-Command.png
[4]:http://www.tecmint.com/wp-content/uploads/2016/10/Remove-All-Files-Except-gz-Files.png
[5]:http://www.tecmint.com/35-practical-examples-of-linux-find-command/
[6]:http://www.tecmint.com/wp-content/uploads/2016/10/Delete-All-Files-Except-Certain-File-Extensions.png
[7]:http://www.tecmint.com/wp-content/uploads/2016/10/Delete-All-Files-Except-Zip-Files-in-Linux.png
[8]:http://www.tecmint.com/wp-content/uploads/2016/10/Delete-All-Files-Except-Few-Files-in-Linux.png
[9]:http://www.tecmint.com/wp-content/uploads/2016/10/DeleteAll-Files-Except-One-File-in-Linux.png
