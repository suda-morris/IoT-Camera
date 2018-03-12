先把rboot.bin放到你的tf卡根目录，并放置一份rboot.sh，这份'rboot.sh'会在rboot启动后自动执行，例如可以是：

boot http://192.168.10.246:8000/firmware/rtthread.bin

后面的是你PC上的启动webserver后，能够访问的链接地址。

然后进入板子的uboot中，输入uboot命令：
setenv bootcmd 'fatload mmc 0 0xA0200000 rboot.bin;go 0xA0200000'
saveenv

即启动时以rboot来启动，第一次启动时请在rboot.bin中输入
wifi cfg your_ssid your_passwd

来配置rboot关联的Wi-Fi网络。然后再次重启，后续每次rboot启动后都会自动去你的PC上下载固件来启动。
