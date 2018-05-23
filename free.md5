linux 下free的使用总结

linux看内存是否够用的标准：只要不用swap的交换空间,就不用担心自己的内存太少.如果常常swap用很多,可能你就要考虑加物理内存

free命令：  free -m   [m\g\k\b..不同计量单位 b (B), -k (KB), -m (MB), -g (GB) and –tera (TB)]
             total       used       free     shared    buffers     cached
Mem:         32059      31304        755          0       1689      15129
-/+ buffers/cache:      14485      17573
Swap:        15999       1589      14410

名词释义

第一部分Mem行:
total 内存总数: 32059M
used 已经使用的内存数: 31304M
free 空闲的内存数: 755M
shared 当前已经废弃不用,总是0
buffers Buffer 块设备缓存区-缓存内存数: 1689M
cached Page 文件缓存-缓存内存数:15129M

> buffers是指用来给块设备做的缓冲大小，他只记录文件系统的metadata以及 tracking in-flight pages.
> cached是用来给文件做缓冲。
> 那就是说：buffers是用来存储，目录里面有什么内容，权限等等。而cached直接用来记忆我们打开的文件

关系：total(32059M) = used(31304M) + free(755M)

第二部分(-/+ buffers/cache):
(-buffers/cache) used内存数：14485 (指的第一部分Mem行中的used – buffers – cached) --- 反映的是被程序实实在在吃掉的内存
(+buffers/cache) free内存数: 17573 (指的第一部分Mem行中的free + buffers + cached) --- 反映的是可以挪用的内存总数

对操作系统来讲是Mem的参数.buffers/cached 都是属于被使用,所以它认为free只有755M.
对应用程序来讲是(-/+ buffers/cach).buffers/cached 是等同可用的,因为buffer/cached是为了提高程序执行的性能,当程序使用内存时,buffer/cached会很快地被使用.

linux下提高磁盘和内存存取效率采取了两种主要Cache方式：Buffer Cache和Page Cache.前者针对磁盘块的读写,后者针对文件inode的读写.这些Cache能有效缩短了 I/O系统调用(比如read,write,getdents)的时间.

虚拟内存 - swap的交换空间  
Linux内核为了提高读写效率与速度，会将文件在内存中进行缓存，这部分内存就是Cache Memory(缓存内存)。即使你的程序运行结束后，Cache Memory也不会自动释放。这就会导致你在Linux系统中程序频繁读写文件后，你会发现可用物理内存变少。当系统的物理内存不够用的时候，就需要将物理内存中的一部分空间释放出来，以供当前运行的程序使用。那些被释放的空间可能来自一些很长时间没有什么操作的程序，这些被释放的空间被临时保存到Swap空间中，等到那些程序要运行时，再从Swap分区中恢复保存的数据到内存中。这样，系统总是在物理内存不够时，才进行Swap交换.

Swap (以KB计)
Total（全部） : 1045500
Used（已用） : 3376
Free（可用） : 1042124
当你看见 buffer/cache 的空闲空间低或者 swap 的空闲空间低，说明内存需要升级了。这意味这内存利用率很高。请注意 shared（共享）内存列应该被忽略 ，因为它已经被废弃了

内存交换 --- 当可用内存少于额定值的时候，就会开会进行交换
cat /proc/meminfo  


