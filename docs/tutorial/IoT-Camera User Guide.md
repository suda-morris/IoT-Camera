# IoT-Camera指南

### 硬件介绍

> - CPU采用FH8620, ARM1176内核, 主频可达450MHz, 芯片内置16MB DDR颗粒，芯片支持H.264编码, 支持720P@45fps实时编码，芯片支持MJPEG编码
> - 板载WiFi采用AP6181模组 (bcm43362芯片)
> - 板载8MB SPI Nor Flash
> - CMOS芯片采用GC1024 sensor



### 安装开发工具
#### Linux环境
* 交叉编译器安装

> Linux下需要安装GNU ARM Embedded Toolchain，工具链下载地址：
> 
> [gcc-arm-none-eabi工具链下载](https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads)

* 安装其他工具

> sudo apt-get install scons libncurses5-dev qemu

####Windows环境
> RT-Thread官方提供了开发环境env，集成了基于mingw的gcc交叉编译工具连，python基础环境，终端调试界面，qemu虚拟环境等


### 编译源码
####添加需要的软件包
1. 打开env软件，工作路径切换到源码目录，运行pkgs --upgrade更新packages list
2. 运行menuconfig,选择RT-Thread online packages下面需要的软件包，比如WiFI
3. 退出menuconfig界面，并保存配置。
4. 运行pkgs --update从网络上拉取最新的软件包到本地
5. 运行scons开始编译工程


###刷机
> FH8620上电后先从SPI Flash中读取uboot执行，然后根据uboot中的启动命令来加载具体的操作系统

* 烧写到SD卡中

> 1. 把rtthread.bin放在SD卡的根目录下（SD卡必须格式化为fat格式，并且要有一个主分区）
> 2. 上电后进入uboot命令行，修改默认启动命令。`setenv bootcmd 'fatload mmc 0 A0000000 rtthread.bin;go A0000000;saveenv'`
> 3. 重启之后即可进入rtthread系统

* 烧写到Flash中

> 1. 把rtthread.bin放在SD卡的根目录下
> 2. 上电后进入uboot命令行，依次执行
	1. fatload mmc 0 A0000000 rtthread.bin
	2. sf probe 0
	3. sf erase 400000 200000
	4. sf write A0000000 400000 200000
	5. setenv bootcmd 'sf probe 0;sf read 0xA0000000 400000 200000;go 0xA0000000'
	6. saveenv


###基本命令使用
1. 查看本地ip地址：ifconfig
2. 连接某个wifi：wifi w0 join <ssid> <password>，w0为接口名
3. 开启热点：wifi ap ap <ssid> <password>，第一个ap为接口名
4. 启动摄像头：mjpeg start
5. 关闭摄像头：mjpeg stop​