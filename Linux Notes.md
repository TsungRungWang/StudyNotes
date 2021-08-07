# Linux学习笔记

==HELP==

`man [CMD]` (manual) 或 `[CMD] --help`

## 路径和目录

`.` 当前目录

`..` 上一级目录

`~` 当前登录用户的用户目录

`*` 通配符, 单独用的话表示某目录下的全部文件

`/` 根目录

绝对路径要以根目录`/`为起始, 相对路径不需要



## 基本操作

`ls <dir>`

- 获取目录下的内容(**l**i**s**t files)
- 默认为当前目录



`cd <dir>`

- 改变当前目录(**c**hange **d**irectory)
- 默认改变到`~`目录
- `cd -` 返回到之前所在目录



`pwd`

- 给出当前**目录**的绝对路径(**p**rint **w**orking **d**irectory)



`mkdir <dir>`

- 新建目录(**m**a**k**e **dir**ectory)



`cp [-r] <src> <dst>`

- 拷贝文件(**c**o**p**y file)
- `-r` 递归拷贝



`./<file>`

- 执行文件



`rm [-rf] <file>`

- 删除文件(**r**e**m**ove file)
- `-r` 递归删除, 否则删除目录的时候不会删除它底下的文件
- `-f` 强制删除, 不需要确认
- 执行`rm`命令的时候要慎重! 慎用`sudo`!! 慎用绝对路径!!!



`rmdir`

- 删除**空**目录
- 可以用来防止一不小心把一大堆文件全删了



`mv`

- 重命名文件, 或者移动文件/目录
- `mv [src] [dst]` 重命名
- `mv [srcdir] [dstdir] [newname]` 移动并重命名





## 杂七杂八的操作

安装软件

从`.deb`包: `# apt install ./<file>.deb`



更新软件

`sudo apt update && apt upgrade`



`reboot` 重启



查看内核温度

`sensors` or

`vcgencmd measure_temp` or

`cat /sys/class/thermal/thermal_zone0/temp`

- 注意不同方法显示出的温度可能略有差别



`date` 打印时间

`which` 查找命令的程序所在的位置

`clear` or `ctrl + L `清空命令行界面





修改显示分辨率

`xrandr` 查看当前分辨率和显示器信息

`cvt 2560 1440` 得到分辨率modeline, 会打印一段这样的文字: 

```
# 2560x1440 59.96 Hz (CVT 3.69M9) hsync: 89.52 kHz; pclk: 312.25 MHz
Modeline "2560x1440_60.00"  312.25  2560 2752 3024 3488  1440 1443 1448 1493 -hsync +vsync
```

`xrandr --newmode 2560x1440 312.25  2560 2752 3024 3488  1440 1443 1448 1493 -hsync +vsync`

- 新建分辨率模式
- `2560x1440`是自己取的分辨率名字
- 后面那一堆数字复制粘贴前面的结果

`xrandr --addmode <displayName> 2560x1440`

- 添加分辨率模式到显示器
- `<displayName>`在`xrandr`打印出来的结果里看
- `2560x1440`是分辨率的名字

`xrandr -s 2560x1440` or

`xrandr --output <displayName> --mode 2560x1440`

- 切换显示分辨率

用这种方法修改的分辨率重启后就会失效, 将上面三个命令添加到`/etc/profile`文件尾部, 就可以了. 



## 有用或者无用的小技巧

`echo <something>` 把"something"再打印一遍







重定向输入输出流: 

`> <file>` 把输出(原本应该打印在屏幕上的内容)写进文件

`< <file>` 从文件读取内容作为输入

`>>` 效果同上, 不过是append追加内容而不是每次写新的

`|` 管道操作: 把左边的输出作为右边的输入. 你就想象左边的内容被输出到一个文本`something`中, 然后右边的操作读取了这个文本内容作为输入内容



`cat` 打印文件内容

`tail` 打印最底下的内容

`tee <file>` 把从管道左边接受的数据写进文件, 同时也在屏幕上输出(?)

`find`

`xdg-open` 打开文件, 用合适的程序



English Names

`/` (forward) slash

`\` backslash

`:` colon

`-` dash

`#` pound symbol



## 权限&用户管理

切换到root权限

`su`

- 第一次切换到root用户需要`sudo passwd root`重置root用户密码



切换回普通用户

- `su <userName>`
- `exit`



添加用户

`adduser <userName>`



删除用户

`userdel [-r] <userName>`



用户重命名

```shell
usermod -l <newNname> <oldName>
mv /home/<oldName> /home/<newName>
usermod -d /home/<newName> <newName>
```

> 用户名重命名之后, 注意检查`~/.local/`目录下, 很多旧文件还是保存着旧用户名作为路径的, 使用`sed -i`改一改就好



## 挂载设备

查看可用设备: `# fdisk -l`



挂载设备: `# mount /dev/sdX1 /mnt`

- `X1`以你实际见到的为准. `X`表示设备的序号, 一般为`a`, `b`, `c`, … ; `1`表示分区. 如果没有数字说明没有分区
- 是将该移动设备中的**所有文件**挂载在`/mnt`目录下, 而不是将该设备挂载在该目录下. 所以最好先在`/mnt`目录下创建一个子目录

> 如果挂载时出现如下报错: 
> 
> `mount: /mnt: wrong fs type, bad option, bad superblock on /dev/sdX1, missing codepage or helper program, or other error.`
> 
> 是因为挂载的文件系统格式不对(Linux默认不支持NTFS文件系统), 需要格式化: 
> 
> `mkfs -t exfat /dev/sdX1`或者`mkfs.exfat /dev/sdX1`
> 
> exFAT文件系统可以兼容Windows和Linux



检查挂载情况: `df -h`

- `df` (**d**isk **f**ree)命令用于显示目前在Linux系统上的文件系统磁盘使用情况统计



取消挂载: `# umount /mnt`

- 注意执行这一句的时候不能位于待取消挂载的路径上, 否则会报错`target is busy`



## 网络

`ssh`

注意, 如果远程服务器有更新, 本地储存的ssh信息便失效了, 此时会有报错: 

`ECDSA host key for <IPaddress> has changed and you have requested strict checking`

解决方法: 本地运行`ssh-keygen -R <IPaddress>`



如果需要使用图形界面, 需要`ssh -Y` (Linux ssh Linux, usually wsl ssh RaspberryPi)

如果是Windows比较麻烦, 需要Xming





`ping`

- 检查网络是否畅通
- 例: `ping sofie.pku.edu.cn`
- 这个命令在Windows Powershell上也可运行



`dig`



`host`

- 查看IP地址



`realpath`

- 给出一个文件的绝对路径



`scp`

- 从服务器下载文件到本地
- `scp [-r] <username>@<IPadress>:<srcdir> <dstdir>`
- 例: `scp lsmtest@222.29.85.5:/workdir/lsm_course/wcr/testReg/run.def D:\Desktop\run.def`
- 注意该命令是在电脑终端运行的, 不是在wsl上运行的(?)
- 如果需要上传文件, 把后面两个参数对调即可
- 如果需要下载整个文件夹, 加上`-r`



## 文本编辑

`vi`/`vim`

- 按`i`进入编辑模式, 按`ESC`退出
- `:wq`, 写入并退出
- `!`, 强制操作







