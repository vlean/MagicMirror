# Make a MagicMirror
let's make a MagicMirror

[![Page Views Count](https://badges.toozhao.com/badges/01EH4NQ1QAMAHTA8GCF37MEKEB/green.svg)](https://badges.toozhao.com/badges/01EH4NQ1QAMAHTA8GCF37MEKEB/green.svg "Get your own page views count badge on badges.toozhao.com")

## 0x00 材料准备

下面的材料都可以万能的淘宝里找到，基础设备上需要用到：

- [x] 树莓派3b
- [x] class 10 mini sd内存卡  
- [x] usb mini手机充电线 (普通的手机充电线都可以)
- [x] 散热小风扇 or 散热片  (选用的`散热片`，但是整体密封后，散热效果差了些)
- [x] 屏幕驱动板/液晶屏/屏幕电源  (选用的`BOE N41屏幕`)
- [x] 原子镜 or 单透贴膜 (选用的`原子镜`，成像效果更好些)

为了让魔镜集成更多的功能，可以配置一些传感器/扩展。

- [x] 温湿度传感器 DHT11
- [x] 声音传感器  (通过高低电平确认是否有声音，敏感度可调)
- [x] 蜂鸣器
- [x] 时钟  DS1302 (树莓派没有内置的时钟，未联网时，时间不准，外接时钟保证时间的准确)
- [ ] 噪声传感器 (可以获取到当前声音分贝)
- [ ] 人体距离感应器  (需要在外框的正面开孔，没有集成)
- [ ] 摄像头 (也需要在外框正面开孔，没做集成)

更多的传感器可以去淘。不过这样还是有些不够智能，亚马逊的`echo`，阿里的`天猫精灵`都做到了语音交互功能，借助语音交互我们可以做更多的事情了:播放音乐、事件提醒、语音聊天、命令控制等等。提供自然语言处理服务的有很多，[百度语音识别](http://yuyin.baidu.com/asr)、[腾讯智能语音服务](https://cloud.tencent.com/product/aai)、[讯飞](http://www.xfyun.cn/sdk/dispatcher)等等。百度的语音服务基本都是在线api的，调用简单还提供了各种语言版本的api文档示例，而且免费调用次数足够满足需求了。讯飞语音合成的声音特别多，但是免费调用的只有两个基本版的，其他有特色的语音合成需要付费。

集成语音服务:
- [ ] 蓝牙音箱/3.5mm音箱
- [ ] usb免驱麦克风
- [ ] 声卡

`树莓派集成语音服务，准备采用蓝牙音箱和普通的usb麦克风。但是蓝牙能连接上，却一直播放不出声音，调试了几天也没找到原因，暂时放弃制作。。so，这方面有了解的朋友SOS...`

外框制作，可选择绘制cad然后交给淘宝去定制边框，也可以自动淘些木板自己动手。材料可以选择亚克力板或者木板。木材推荐白蜡、榉木、红松这些，它们的特点是结疤少，纹路细腻，制作出来的效果很赞，打磨后不用上漆就很光滑自然,我选用的榉木,后面可以看下效果。自己制作的话，没有木工经验的建议去找个木工俱乐部，借助机械化的设备来做，省时省力还省钱 :)。

- [ ] 木制外框CAD制图/CAD加工定制
- [x] 设计图纸/自制


## 0x01 外框制作和组装


- 15.6寸屏幕:  228x370x4mm
- 原子镜: 200x350x6mm

外框设计注意内框的尺寸、边距、外框的尺寸，避免返工。同时预留好电源出口，散热孔。

用手工工具做的话，练了几天发现总是钜不直，所以最后找了家莘庄的木工俱乐部，不得不令人感慨下机械加工的精度和速度，特别感谢周师傅的帮忙。下面是环境：

![场地](/public/场地.jpg) ![场地2](/public/场地2.jpg)

简单做下使用了4个`L`型的结构，然后用木工胶拼接到一块。

![L](/public/L.jpeg)

外框平面图：

`L`型切面图:

4个`L`的边缘45度切割，使用木工胶粘贴到一起，用木工夹固定，视胶干的情况等待半小时到一天。

![加固](/public/加固.jpeg)

这次组装的时候布线设计的不好，很多线都太长了，不得不盘起来再固定，这样又占用了空间还又不小的安全隐患。所以hdmi线和usb充电线建议买短点。
为了减少电源接口，把屏幕的电源线拆开，将usb充电器嵌入到里面。这样只需要提供一个电源出口就可以了。要注意的是屏幕的电源线是三相的，其中同色的是零线、火线，另一个是地线。我这的地线是红色的，所以拼接的时候试了好几次，有试电笔的话会简单很多。

![布线](/public/布线.jpeg)


另外要注意cpu和gpu散热，我处理的散热不太好，只用了散热片，在密封后，cpu占用过高时温度上升过快，不到一分钟就会直接宕机。可以考虑风扇和散热片同时使用。不过大部分情况cpu占用在10%左右，几次温度快速升高都在重复切断电源、开机启动`MagicMirrors`之后发生的。



## 0x02 系统服务安装

**树莓派系统**

[Raspberry Pi System](https://www.raspberrypi.org/downloads/)

**液晶屏&驱动板**

这个比较幸运，从淘宝上买的N41屏幕，在另一家买的N41的驱动板，安装后一次点亮。比较坑的是当时没留意把驱动板放到铁器上面，驱动板后面的电容烧毁了，又找卖家修了下。

**ssh免密登录**

**安装nginx/php服务**
[composer说明](https://getcomposer.org/download/)
```bash
sudo apt-get install php7.0 nginx -y  #也可以编译安装

#安装composer
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
sudo mv composer.pher /usr/local/bin/composer
```

**[内网穿透服务](https://github.com/fatedier/frp)**

**[自动部署服务](https://github.com/NetEaseGame/git-webhook)**

内网穿透+自动部署，就可以实现推代码的同时更新树莓派上指定的代码了。
内网穿透需要一台外网主机当做服务器，树莓派当客户端来连接。
自动部署服务部署在外网主机上，将ssh公钥放到树莓派上。通过ssh来远程执行部署命令。

**[树莓派仪盘表](http://maker.quwj.com/project/10)**


**[wifi自动连接配置](http://blog.csdn.net/geekcome/article/details/7325198)**

配置几个ssid和密码就不用通过键鼠来连接wifi了。

## 0x03 配置传感器

这边配置了`声音传感器`、`温湿度传感器`、`时钟`和`蜂鸣器`。代码都是从网上拷贝后修改的，要注意的是GPIO针脚。可以参考这篇[树莓派GPIO引脚对照表](http://shumeipai.nxez.com/raspberry-pi-pins-version-40)来配置。

[树莓派GPIO扩展库](http://blog.csdn.net/xukai871105/article/details/12684617)

**温湿度传感器**

Github上找的这个[DHT_Python](https://github.com/szazo/DHT11_Python)。适用前安装python GPIO扩展：

```bash
#python2
sudo apt-get install python-rpi.gpio

#python3
sudo apt-get install python3-rpi.gpio
```

修改dht11_example.py
```python
import RPi.GPIO as GPIO
import dht11

GPIO.setwarnings(False)
GPIO.setmode(GPIO.BCM)
GPIO.cleanup()

# 适用BCM下的针脚  注意替换
instance = dht11.DHT11(pin = 14)
result = instance.read()

def saveTemperAndHum(temper,humi):
    #...
    #存储逻辑，留待提供接口
    #我存了7天的历史数据每30分钟采样一次,做历史曲线

if result.is_valid():
    print("Temperature: %d C" % result.temperature)
    print("Humidity: %d %%" % result.humidity)
    saveTemperAndHum(result.temperature,result.humidity)
else:
    print("Error: %d" % result.error_code)

```

**时钟 DS1302**

参考的[WiringPi](https://github.com/WiringPi/WiringPi/blob/master/examples/ds1302.c)和[为树莓派3B添加一个实时时钟DS1302](http://blog.csdn.net/Eric_lmy/article/details/52754432),主要为了修正系统时间，完全参考的配置，不再写了。

**蜂鸣报警器**

这个比较简单，通常和其他传感器配合使用，如声音、震动、红外灯，检测到特定行为可以触发提醒。它是out针脚低电平触发报警。 其他参考[蜂鸣器](https://baike.baidu.com/item/%E8%9C%82%E9%B8%A3%E5%99%A8)

**声音传感器**

参考[树莓派GPIO入门07-利用声音传感器制作声控灯](http://blog.mangolovecarrot.net/2015/05/22/raspi-study07/)

```python
import RPi.GPIO
import time
import os

# 声音感应器OUT口连接的GPIO口,BCM端口
SENSOR = 4

RPi.GPIO.setmode(RPi.GPIO.BCM)

# 指定GPIO4（声音感应器的OUT口连接的GPIO口）的模式为输入模式
# 默认拉高到高电平，低电平表示OUT口有输出
RPi.GPIO.setup(SENSOR, RPi.GPIO.IN, pull_up_down=RPi.GPIO.PUD_UP)


try:
    while True:
        # 检测声音感应器是否输出低电平，若是低电平，表示有声音
        if (RPi.GPIO.input(SENSOR) == 0):
            # 唤醒屏幕
            os.system("export DISPLAY=:0.0 && xset dpms force on")
            time.sleep(0.5)
        
        #限制频率防止占用过高cpu
        time.sleep(0.001)

except KeyboardInterrupt:
    pass

RPi.GPIO.cleanup()

```


## 0x04 搭建MagicMirror

**安装MagicMirror**

下面的安装有时会遇到一直卡在electron扩展的安装，看github里有很多提示无法安装的问题，我强制指定了下源，等待了许久安装成功了。
但新版的`MagicMirror`使用了模拟器，相比打开chrome访问固定网址，占用的cpu更高，发热比较大，所以最后还是采用了原始的方式。

```bash
#debian 7及以下使用下面命令
sudo apt-get install python-software-properties
#debian 8使用下面命令
sudo apt-get install software-properties-common

sudo add-apt-repository ppa:gias-kay-lee/npm
sudo apt-get update
sudo apt-get install nodejs npm -y
sudo npm install -g nrm
sudo npm install -g nvm
sudo nrm use taobao #切换到国内源

#Automatic Installer (Raspberry Pi Only!)
bash -c "$(curl -sL https://raw.githubusercontent.com/MichMich/MagicMirror/master/installers/raspberry.sh)"
#安装MagicMirrors
cd ~/MagicMirrors
npm install

#安装过程中如果一直卡在安装electron扩展，可以退出，执行下面命令
#删除文件
rm -rf node_modules/electron
#设置源
npm i electron --registry=https://registry.npm.taobao.org
#具体可以参考这个issue
#https://github.com/MichMich/MagicMirror/issues/1005

#or 
sudo npm install -g electron@1.7.6

#运行MagicMirrors
npm start
```

最后在[HelloWk/MagicMirror](https://github.com/HelloWk/MagicMirror)的基础上修改了下天气调用的api、新闻及日历事件。




**supervisor管理脚本**
[配置参考](http://blog.csdn.net/xyang81/article/details/51555473)

supervisor配置

```ini
#supervisor.conf配置文件
[supervisord]
logfile=/tmp/supervisor.log
logfile_maxbytes=50MB
logfile_backups=10
loglevel=info

[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface
[supervisorctl]
serverurl=unix:///tmp/supervisor.sock

[unix_http_server]
file=/tmp/supervisor.sock

[include]
files = conf/*.ini

#管理脚本
[program:mm]
command=/home/pi/start.sh 
autostart=true
startsecs=1
autorestart=true
startretries=0
user=pi
redirect_stderr=true
stdout_logfile=/tmp/super_mm.log
```

`start.sh`脚本内容
```bash
#!/bin/bash
#启动后如有提示默认浏览器等弹框，可以通过增加--disable-***参数来控制
nohup chromium-browser –start-maximized –start-fullscreen --kiosk --incognito  http://localhost > /tmp/chrome.log &
```

没有设置屏幕常亮，正常30分钟后，屏幕会关闭。通过声音传感器来监听声音，如果有声音就唤醒屏幕。


![start](/public/start.jpeg) ![MagicMirror](/public/MagicMirror.jpeg)
## 0x05 结语

以上结束。

## 0xFF 参考资料

> [MagicMirror](https://github.com/MichMich/MagicMirror)
>
> [树莓派GPIO引脚对照表](http://shumeipai.nxez.com/raspberry-pi-pins-version-40)
>
> [配置wifi连接](http://blog.csdn.net/geekcome/article/details/7325198)
>
> [树莓派GPIO入门07-利用声音传感器制作声控灯](http://blog.mangolovecarrot.net/2015/05/22/raspi-study07/)
