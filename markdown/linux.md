# Linux安装工具

## 检查网络状态

```shell
hadoop@ubuntu:~$ ping www.baidu.com
PING www.a.shifen.com (110.242.68.3) 56(84) bytes of data.
64 字节，来自 110.242.68.3 (110.242.68.3): icmp_seq=1 ttl=128 时间=32.4 毫秒
64 字节，来自 110.242.68.3 (110.242.68.3): icmp_seq=2 ttl=128 时间=34.3 毫秒
64 字节，来自 110.242.68.3 (110.242.68.3): icmp_seq=3 ttl=128 时间=59.1 毫秒
^C
--- www.a.shifen.com ping 统计 ---
已发送 3 个包， 已接收 3 个包, 0% 包丢失, 耗时 2003 毫秒
rtt min/avg/max/mdev = 32.408/41.925/59.103/12.169 ms
```

## 配置apt源

```shell
sudo vi /etc/apt/sources.list

# 默认注释了源码仓库，如有需要可自行取消注释
deb https://mirrors.ustc.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ focal-security main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-security main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-updates main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-backports main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.ustc.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
```

## 更新apt软件包信息

```shell
hadoop@ubuntu:~$ sudo apt-get update
```

## 安装Tools

```shell
apt-get install build-essential    # build-essential packages, include binary utilities, gcc, make, and so on
apt-get install man                # on-line reference manual
apt-get install gcc-doc            # on-line reference manual for gcc
apt-get install gdb                # GNU debugger
apt-get install git                # revision control system
apt-get install libreadline-dev    # a library used later
apt-get install libsdl2-dev        # a library used later
apt-get install llvm               # llvm project, which contains libraries used later
```

# GCC

## 编译选项

```shell
-E        Preprocess only; do not compile, assemble or link. 仅预处理;请勿编译、汇编或链接。
-S        Compile only; do not assemble or link. 仅编译;请勿汇编或链接。
-c        Compile and assemble, but do not link. Compile and assemble, but do not link.
-o <file>  Place the output into <file>.

```





1. 预处理

   ```shell
   hadoop@ubuntu:~/code$ ls
   test.c
   hadoop@ubuntu:~/code$ gcc -E test.c -o test.i
   hadoop@ubuntu:~/code$ ls
   test.c  test.i
   ```

2.  编译

   ```shell
   hadoop@ubuntu:~/code$ ls
   test.c  test.i
   hadoop@ubuntu:~/code$ gcc -S test.i -o test.s
   hadoop@ubuntu:~/code$ ls
   test.c  test.i  test.s
   ```

3.  汇编

   ```shell
   hadoop@ubuntu:~/code$ ls
   test.c  test.i  test.s
   hadoop@ubuntu:~/code$ gcc -c test.s -o test.o
   hadoop@ubuntu:~/code$ ls
   test.c  test.i  test.o  test.s
   ```

4.  链接

   ```shell
   hadoop@ubuntu:~/code$ ls
   test.c  test.i  test.o  test.s
   hadoop@ubuntu:~/code$ gcc test.o -o test
   hadoop@ubuntu:~/code$ ls
   test  test.c  test.i  test.o  test.s
   ```

   

# Ubuntu UEFI & BIOS U盘双启动

## 新建虚拟机

1. CD/DVD
   1. 选择UbuntoISO文件位置 启动时连接

2. USB控制器
   1. USB兼容性   USB 3.0

3. 添加硬盘
   1. 计算机管理 - > 磁盘管理  查看U盘编号
   2. 虚拟机设置->添加->硬盘->SCSI->使用物理磁盘->选择U盘  使用整个磁盘
4. 设置UEFI启动
   1. 虚拟机设置->选项->高级->固件类型->UEFI

## U盘分区并安装系统

1. 开机启动gparted

2. 新建GPT分区表  driver->create partition table  

3. U盘分区

   1. ext4: 系统分区 挂载到根目录 sda1

   2. fat32: /boot, esp 启动 sda2

   3. swap: 交换分区

   4. 设置bios_grub标记  最后剩1M就行

      ```shell
      hadoop@ubuntu:~$ sudo gdisk /dev/sda
      GPT fdisk (gdisk) version 1.0.5
      
      Partition table scan:
        MBR: MBR only
        BSD: not present
        APM: not present
        GPT: not present
      
      
      ***************************************************************
      Found invalid GPT and valid MBR; converting MBR to GPT format
      in memory. THIS OPERATION IS POTENTIALLY DESTRUCTIVE! Exit by
      typing 'q' if you don't want to convert your MBR partitions
      to GPT format!
      ***************************************************************
      
      Command (? for help): n
      Partition number (3-128, default 3): 
      First sector (34-104857566, default = 104855552) or {+-}size{KMGTP}: 
      Last sector (104855552-104857566, default = 104857566) or {+-}size{KMGTP}: 
      Current type is 8300 (Linux filesystem)
      Hex code or GUID (L to show codes, Enter = 8300): EF02
      Changed type of partition to 'BIOS boot partition'
      
      Command (? for help): p
      Disk /dev/sda: 104857600 sectors, 50.0 GiB
      Model: VMware Virtual S
      Sector size (logical/physical): 512/512 bytes
      Disk identifier (GUID): 0984FF1D-54D9-4AFE-8015-7C639179D0EB
      Partition table holds up to 128 entries
      Main partition table begins at sector 2 and ends at sector 33
      First usable sector is 34, last usable sector is 104857566
      Partitions will be aligned on 2048-sector boundaries
      Total free space is 2014 sectors (1007.0 KiB)
      
      Number  Start (sector)    End (sector)  Size       Code  Name
         1            2048         1050623   512.0 MiB   0700  Microsoft basic data
         2         1050624         1052671   1024.0 KiB  8300  Linux filesystem
         3       104855552       104857566   1007.5 KiB  EF02  BIOS boot partition
         5         1052672       104855551   49.5 GiB    8300  Linux filesystem
      
      Command (? for help): w
      
      Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
      PARTITIONS!!
      
      Do you want to proceed? (Y/N): y
      OK; writing new GUID partition table (GPT) to /dev/sda.
      Warning: The kernel is still using the old partition table.
      The new table will be used at the next reboot or after you
      run partprobe(8) or kpartx(8)
      The operation has completed successfully.
      
      ```

      

4. 双击桌面Install Ubunto安装系统 安装类型其他选项

5. 添加BIOS启动

   1. 切换固件启动类型为BIOS启动虚拟机

   2. Try Ubuntu

   3. 终端

      ```shell
      " 系统挂载分区/dev/sda1
      $ sudo mount /dev/sda1 /mnt
      $ sudo grub-install --target=i386-pc --recheck --boot-directory=/mnt/boot /dev/sda
      
      heck --boot-directory=/mnt/boot /dev/sda
      Installing for i386-pc platform.
      Installation finished. No error reported.
      " 报错检查bios_grub标记
      ```

6. 查看启动方式:

   ```shell
   # [ -d /sys/firmware/efi ] && echo UEFI || echo BIOS
   ```


## 参考

[把 Ubuntu 装到U盘里随身携带，并同时支持 BIOS 和 UEFI 启动 - GGAutomaton 的博客 - 洛谷博客 (luogu.com.cn)](https://www.luogu.com.cn/blog/GGAutomaton/portable-ubuntu-bootable-in-UEFI-and-BIOS)



# ANSI控制字符

格式: `\e[options1[;options2[...]]m`

options

```
0: 黑  
1: 红
2: 绿
3: 黄
4: 蓝
5: 紫
6: 绿 
7: 白 
+30 深色字体 +90  亮色字体
+40 深色背景 +100 亮色背景


\033[0m 关闭所有属性 
\033[1m 设置高亮度 
\033[4m 下划线 
\033[5m 闪烁 
\033[7m 反显 
\033[8m 消隐 
\033[30m -- \033[37m 设置前景色 
\033[40m -- \033[47m 设置背景色 
\033[nA 光标上移n行 
\033[nB 光标下移n行 
\033[nC 光标右移n行 
\033[nD 光标左移n行 
\033[y;xH设置光标位置 
\033[2J 清屏 
\033[K 清除从光标到行尾的内容 
\033[s 保存光标位置 
\033[u 恢复光标位置 
\033[?25l 隐藏光标 
\033[?25h 显示光标
```

# TMUX基本使用

## 配置

​	``cd ~``回到家目录``vim .tmux.conf``添加如下内容

​	bind-key c new-window -c "#{pane_current_path}"
​	bind-key % split-window -h -c "#{pane_current_path}"
​	bind-key '"' split-window -c "#{pane_current_path}"

​	这三行设置使 `tmux` 在创建新窗口/窗格时“记住”当前窗格的当前工作目录。

​	最大化终端窗口大小，然后使用 `tmux` 在单个屏幕内创建多个正常大小的终端。	例如，您可以同时编辑不同目录中的不同文件。您可以在不同的终端中编辑它	 们、编译它们或在另一个终端中执行其他命令，而无需来回打开和关闭源文件。您可以上下滚动 `tmux` 终端中的内容

## 基本使用

1. 启动tmux：在终端中输入tmux并按下回车键即可启动tmux。
2. 创建新会话：启动tmux后，会自动创建一个新的会话。你可以使用快捷键Ctrl+b，然后按下c来创建一个新的会话。
3. 切换会话：使用快捷键Ctrl+b，然后按下n来切换到下一个会话，或按下p来切换到上一个会话。
4. 分割窗格：使用快捷键Ctrl+b，然后按下%来垂直分割当前窗格，或按下"来水平分割当前窗格。
5. 切换窗格：使用快捷键Ctrl+b，然后按下方向键来切换到相邻的窗格。
6. 关闭窗格：使用快捷键Ctrl+b，然后按下x来关闭当前窗格。
7. 滚动窗格：使用快捷键Ctrl+b，然后按下[进入复制模式，然后可以使用方向键或Page Up/Page Down键来滚动窗格内容。
8. 退出tmux：在tmux会话中，输入exit命令并按下回车键即可退出tmux。

# 卸载Firefox

```shell
$ dpkg --get-selections |grep firefox
```

