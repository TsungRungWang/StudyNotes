# Linux学习笔记

==HELP!!!==

`man [CMD]` (manual) 或 `[CMD] --help`



`.` 当前目录

`..` 上一级目录

`~` 当前登录用户的用户目录

`*` 通配符, 单独用的话表示某目录下的全部文件

`/` 根目录

绝对路径要以根目录`/`为起始, 相对路径不需要



`clear`清空命令行界面



`ls` 获取当前目录下的内容(**l**i**s**t files)

`cd [dir]` 改变当前目录(**c**hange **d**irectory)

`pwd` 给出当前目录的绝对路径(**p**rint **w**orking **d**irectory)

`mkdir [dir]` 建立新的目录(**m**a**k**e **dir**ectory)

`cp [-r] [src] [dst]` 拷贝文件(**c**o**p**y file)

- `-r` 递归拷贝

`./[FILENAME]` 执行文件

`rm [-rf] [FILE]` 删除文件(**r**e**m**ove file). 

- `-r` 递归删除, 否则删除目录的时候不会删除它底下的文件(?)
- `-f` 强制删除, 不需要确认
- 执行`rm`命令的时候要慎重! 慎用`sudo`!! 慎用绝对路径!!!

`mv` 重命名文件, 或者移动文件/目录

- `mv [src] [dst]` 重命名
- `mv [srcdir] [dstdir] [newname]` 移动并重命名



更新软件

`sudo apt update && apt upgrade`





`ping` 查看ip地址

- 实际上是用来检查网络是否畅通的
- 例: `ping sofie.pku.edu.cn`
- 这个命令在Linux和Windows Powershell上均可运行



`realpath`



`scp` 从服务器下载文件到本地

- `scp [-r] [username]@[ipadress]:[srcdir] [dstdir]`
- 例: `scp lsmtest@222.29.85.5:/workdir/lsm_course/wcr/testReg/run.def D:\Desktop\run.def`
- 注意该命令是在电脑终端运行的, 不是在wsl上运行的
- 如果需要上传文件, 把后面两个参数对调即可
- 如果需要下载整个文件夹, 加上`-r`



`vi`

- 创建新文件
