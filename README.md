# xxfpm
FCGI Process Manager

这是一个我在2011年写的跨平台FastCGI进程管理器，可用作php的进程管理。

## 使用方式

```
Usage: xxfpm path [-n number] [-i ip] [-p port]
Manage FastCGI processes.

-n, –number number of processes to keep
-i, –ip ip address to bind
-p, –port port to bind, default is 8000
-u, –user start processes using specified linux user
-g, –group start processes using specified linux group
-r, –root change root direcotry for the processes
-h, –help output usage information and exit
-v, –version output version information and exit
```

## 相关博文

http://xiaoxia.org/2011/02/01/xxfpm-wrote-a-fastcgi-process-manager/


xxfpm: 写了一个小巧的FastCGI进程管理器

经测试，支持Win32和Linux-x86平台。对于用php的人，有了这个东西来维护一定数量的进程，就能制服经常崩溃退出的php-cgi啦！！！

Usage: xxfpm path [-n number] [-i ip] [-p port]
Manage FastCGI processes.

-n, –number number of processes to keep
-i, –ip ip address to bind
-p, –port port to bind, default is 8000
-u, –user start processes using specified linux user
-g, –group start processes using specified linux group
-r, –root change root direcotry for the processes
-h, –help output usage information and exit
-v, –version output version information and exit
第一个写得比较标准的终端应用程序，我是看了cygwin的里的一些源代码，然后学会了如何使用getopt，算是写得比较标准的，但是代码也不短。

使用例子：
xxfpm z:/php5/php-cgi.exe -n 5 -p 8080

有人问，如何给程序加入参数？这个不难，使用双引号即可，路径要用”/”而不用”\”。例如要指定php.ini的路径，可以用下面例子：
xxfpm “z:/php5/php-cgi.exe -c z:/php5/php.ini” -n 5 -i 127.0.0.1 -p 8080


如何维护进程：

Windows上使用CreateProcess创建进程，使用WaitForSingleObject等待进程结束；Linux上使用fork和execl创建进程，使用waitpid等待进程结束。Linux的版本多了在创建子进程的时候可以设置进程限制，能够以受限用户方式来运行。

当进程管理器被关闭的时候，它所创建的所有子进程也必须被关闭。Windows上使用JobObject这个东西来把子进程与管理器的进程产生关联，感谢iceboy提供的资料！Linux上通过捕捉关闭信号，然后给所有子进程发送SIGTERM来结束子进程。详见源代码！！！
