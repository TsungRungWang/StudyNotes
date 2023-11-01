# Personal Website



## NGINX

两个常用的Web Server, Apache和Nginx. 这里我用的是Nginx



安装Nginx

`sudo apt install nginx`

帮助文档

`nginx -h`. 注意`--help`不能用



启动Nginx服务

|                           | Systemd (Most Modern Linux Distributions) | Traditional Init Systems (SysVinit or Upstart) |
| ------------------------- | ----------------------------------------- | ---------------------------------------------- |
| General Syntax            | `sudo systemctl _ nginx`                  | `sudo service nginx _`                         |
| Start Nginx               | `sudo systemctl start nginx`              | `sudo service nginx start`                     |
| Stop Nginx                | `sudo systemctl stop nginx`               | `sudo service nginx stop`                      |
| Restart Nginx             | `sudo systemctl restart nginx`            | `sudo service nginx restart`                   |
| Reload Nginx \*           | `sudo systemctl reload nginx`             | `sudo service nginx reload`                    |
| Check the status of Nginx | `sudo systemctl status nginx`             | `sudo service nginx status`                    |

\* to apply configuration changes without downtime

注意两个命令的语序差异





在Nginx中被展示的网页储存在`/var/www/html`中, 当然你也可以自己指定

实际需要展示哪些网页, 需要在`/etc/nginx/sites-available`中添加server block, 并把你想生效的server block链接(symbolic link)到`/etc/nginx/sites-enabled`中: 

```bash
sudo ln -s /etc/nginx/sites-available/mywebsite /etc/nginx/sites-enabled/
```



server block的语法大致如下

```nginx
server {
    listen [your_ipv6_address]:80; # ip_address:port. Can I have more than 1 address to listen to?
    server_name your_domain.com www.your_domain.com; # This decides from where do you want your server to be accessed. Wildcard _ possible

    root /var/www/html; # Change this to your website's root directory
    index index.html; # Default file to serve when the root of the website accessed

    location / {
        try_files $uri $uri/ =404;
    }
}
```



测试配置

`sudo nginx -t`



如何组织文件, ChatGPT的建议:

Organize your website files within the `/var/www/html` directory. For example, you can create subdirectories like `/var/www/html/css`, `/var/www/html/images`, and so on to keep your files organized.

当然, 如果用CMS的话, 那它都帮你组织好了, 不需要自己操心



## DDNS

### ddclient

在树莓派上使用ddclient: 参考GitHub页面中的步骤一步一步操作即可

https://github.com/ddclient/ddclient

安装并配置ddclient

先cd到`~`, clone到本地: `git clone https://github.com/ddclient/ddclient`

cd进`ddclient`, run `./autogen`

*注意这里可能出现一些程序未安装导致的报错, 而且报错中不会直接提示你这个问题, 把报错信息放在网上搜索一下即可 (run `sudo apt install autoconf`)*

run: 

```bash
./configure \
    --prefix=/usr \
    --sysconfdir=/etc/ddclient \
    --localstatedir=/var
make
make VERBOSE=1 check
sudo make install
```

edit `/etc/ddclient/ddclient.conf`

在里面加入配置内容. 



接下来按照GitHub页面中的操作即可:

copy systemd

```bash
cp sample-etc_systemd.service /etc/systemd/system/ddclient.service
```

enable automatic startup when booting

```bash
systemctl enable ddclient.service
```

start the first time by hand

```bash
systemctl start ddclient.service
```



查看ddclient运行状态

```bash
sudo systemctl status ddclient
```

### Cloudflare

在`ddclient.conf`中加入如下内容

```
usev6=ifv6, ifv6=eth0
protocol=cloudflare
zone=congrong.wang
login=token
password=<my-api-token>
congrong.wang
```

设置格式在安装好后的`ddclient.conf`中有, 搜索`cloudflare`即可



可以参考这篇

https://www.reddit.com/r/SelfHosting/comments/16wnu3s/ddclient_and_cloudflare_dynamic_dns/

and this:

https://www.reddit.com/r/selfhosted/comments/cldqix/ddclient_wont_update_cloudflare/

或者这篇, 但是这篇是完全自写, 没有利用ddclient: https://www.rohanjain.in/cloudflare-ddns/. 以后有时间挖掘一下



在`ddclient.conf`中加入如下内容

```
usev6=ifv6, ifv6=eth0
protocol=dyndns2
server=dynv6.com
login=none
password=<password>
<you_hostname>,<your_another_hostname>
```

其中, `usev6`而非`use`, 否则不会更新IPv6地址

`if=`后面的内容可以通过运行`ifconfig -a`查看

后面的连接信息可以在dynv6.com页面中的instruction中查看



### 番外: 如何移除程序

见: https://askubuntu.com/questions/187888/what-is-the-correct-way-to-completely-remove-an-application





### 背景知识: 查看Ubuntu系统使用情况

> Me: Is there a command for Ubuntu to learn the current status like memory usage of the machine?
>
> ChatGPT: Yes, you can use various commands in Ubuntu to check the current status of your machine, including memory usage. Here are some commonly used commands:
>
> 1. **`free`**: This command displays information about both physical and swap memory usage. Running `free -h` will provide human-readable output with memory sizes in MB and GB.
>
>    ```
>    free -h
>    ```
>
> 2. **`top`**: The `top` command provides real-time information about system processes, including CPU and memory usage. It's an interactive command, and you can exit by pressing `q`.
>
>    ```
>    top
>    ```
>
> 3. **`htop`**: Similar to `top`, `htop` is an interactive process viewer that provides a more user-friendly and feature-rich interface for monitoring system resources.
>
>    ```
>    htop
>    ```
>
> 4. **`vmstat`**: This command reports information about processes, memory, paging, block I/O, traps, and CPU activity.
>
>    ```
>    vmstat
>    ```
>
> 5. **`iotop`**: If you want to monitor disk I/O in real-time, you can use `iotop` to see which processes are reading/writing to your disk.
>
>    ```
>    iotop
>    ```
>
> 6. **`df`**: To check disk space usage on your system, use the `df` command. It displays information about disk partitions and their usage.
>
>    ```
>    df -h
>    ```
>
> 7. **`du`**: To check disk usage in a specific directory, you can use the `du` command. For example, to check the size of a directory named "myfolder," use:
>
>    ```
>    du -h myfolder
>    ```
>
> These commands should help you monitor various aspects of your Ubuntu system, including memory usage. Choose the one that best suits your needs and preferences.



### 背景知识: 如何用ssh传输文件

```bash
scp <local_file> <username>@<hostname>:<remote_directory>
```

或者使用[WinSCP](https://winscp.net)





## https: letsencrypt

`sudo apt install certbot`



Along with the "Let's Encrypt" package, this command also installs the "certbot.timer" utility for automatic certificate renewal. It checks the validity of SSL certificates in the system twice a day and extends those that expire in the next 30 days. To make sure that it is running, enter

检查证书自动更新程序

```bash
sudo systemctl status certbot.timer
```



建立一个(或多个)域名的certificate

```bash
sudo certbot certonly --standalone --agree-tos --preferred-challenges http -d domain-name.com
```

上面的语句好像不能用了, 试试

```bash
 sudo certbot certonly --nginx -w /var/www/wp -d domain-name.com
```



如果需要添加多个的话就`-d domain-name-1.com -d domain-name-2.com`

成功之后会提示:

```bash
Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/domain-name.com/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/domain-name.com/privkey.pem
```



安装过程中要运行这一句但是我也不知道这个包是干啥的

`sudo apt install python3-certbot-nginx`



建立一个wildcard ssl

```bash
sudo certbot certonly --manual --agree-tos --preferred-challenges dns -d domain-name.com -d *.domain-name.com
```





列出所有certificate

```
sudo certbot certificates
```



interactively remove and clean up unwanted / deprecated domains

这会进入一个互动界面

```
sudo certbot delete
```



## MySQL

```mysql
CREATE DATABASE yourdbname;
CREATE USER 'yourusername'@'localhost' IDENTIFIED BY 'yourpassword';
GRANT ALL PRIVILEGES ON yourdbname.* TO 'yourusername'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```



登入MySQL

```mysql
mysql -u root -p
```

当然你也可以创建一个自己的用户, 不是每次都要用`root`登入的



查看所有的数据库

```mysql
show databases;
```

其中有如下内置数据库:

```mysql
| information_schema |
| mysql              |
| performance_schema |
| sys                |
```



## WordPress



