how to skip password while climbing GFW in linux
2012-07-20

首先安装 putty-tools, 其中包含了plink
    sudo apt-get install putty-tools

接着设置好浏览器的SOCKS代理:
    127.0.0.1, 1080

然后用下面的命令试验下:
    plink -ssh -C -D PORT -pw PASSWORD USER@yoursite.com

如果是第一次连接, 会要求你确认远程主机的指纹. 输入y即可.

连接成功后, 应该会打印如下内容:
    Using username "spig".
    Last login: Fri Jun  1 00:02:55 2012 from 122.224.76.38

现在密码可以作为参数嵌入在命令中了, 那么就可以将命令写入脚本, 随时调用.
我创建了如下的脚本, 取名为 fuckgfw:

    plink -ssh -C -D PORT -pw PASSWORD user@example.com &

最后的&是为了让进程后台化.

给这个文件增加可执行权限:
    chmod a+x fuckgfw

然后放在某个系统目录下:
    sudo cp fuckgfw /bin

以后如果要翻墙, 只需要在任意控制台敲  fuckgfw 就OK了.
