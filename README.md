# MagicMirror
let's make a MagicMirror

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
- [x] 声音传感器  DS1302 (通过高低电平确认是否有声音，敏感度可调)
- [x] 蜂鸣器
- [x] 时钟  (树莓派没有内置的时钟，未联网时，时间不准，外接时钟保证时间的准确)
- [ ] 噪声传感器 (可以获取到当前声音分贝)
- [ ] 人体距离感应器  (需要在外框的正面开孔，没有集成)
- [ ] 摄像头 (也需要在外框正面开孔，没做集成)

更多的传感器可以去淘。不过这样还是有些不够智能，亚马逊的`echo`，阿里的`天猫精灵`都做到了语音交互功能，借助语音交互我们可以做更多的事情了:播放音乐、事件提醒、语音聊天、命令控制等等。提供自然语言处理服务的有很多，[百度语音识别](http://yuyin.baidu.com/asr)、[腾讯智能语音服务](https://cloud.tencent.com/product/aai)、[讯飞](http://www.xfyun.cn/sdk/dispatcher)等等。百度的语音服务基本都是在线api的，调用简单还提供了各种语言版本的api文档示例，而且免费调用次数足够满足需求了。讯飞语音合成的声音特别多，但是免费调用的只有两个基本版的，其他有特色的语音合成需要付费。

集成语音服务:
- [ ] 蓝牙音箱/3.5mm音箱
- [ ] usb免驱麦克风
- [ ] 声卡

`树莓派集成语音服务，准备采用蓝牙音箱和普通的usb麦克风。但是蓝牙能连接上，却一直播放不出声音，调试了几天也没找到原因。so，Sos...`

外框制作，可选择绘制cad然后交给淘宝去定制边框，也可以自动淘些木板自己动手。材料可以选择亚克力板或者木板。木材推荐白蜡、榉木、红松这些，它们的特点是结疤少，纹路细腻，制作出来的效果很赞，打磨后不用上漆就很光滑自然,我选用的榉木,后面可以看下效果。自己制作的话，没有木工经验的建议去找个木工俱乐部，借助机械化的设备来做，省时省力还省钱 :)。

- [ ] 木制外框CAD制图/CAD加工定制
- [x] 设计图纸/自制


## 0x01 外框制作和组装


- 15.6寸屏幕:  228x370x4mm
- 原子镜: 200x350x6mm

外框设计注意内框的尺寸、边距、外框的尺寸，避免返工。
同时预留好电源出口，散热孔。

简单做下使用了4个`L`型的结构，然后用木工胶拼接到一块。

外框平面图：

`L`型切面图:

4个`L`的边缘45度切割，使用木工胶粘贴到一起，用木工夹固定，视胶干的情况等待半小时到一天。



这次组装的时候布线设计的不好，很多线都太长了，不得不盘起来再固定，这样又占用了空间还又不小的安全隐患。所以hdmi线和usb充电线建议买短点。
为了减少电源接口，把屏幕的电源线拆开，将usb充电器嵌入到里面。这样只需要提供一个电源出口就可以了。要注意的是屏幕的电源线是三相的，其中同色的是零线、火线，另一个是地线。我这的地线是红色的，所以拼接的时候试了好几次，有试电笔的话会简单很多。

另外要注意cpu和gpu散热，我处理的散热不太好，只用了散热片，在密封后，cpu占用过高时温度上升过快，不到一分钟就会直接宕机。可以考虑风扇和散热片同时使用。不过大部分情况cpu占用在10%左右，几次温度快速升高都在重复切断电源、开机启动`MagicMirrors`之后发生的。



## 0x02 系统服务安装

**树莓派系统**

[Raspberry Pi System](https://www.raspberrypi.org/downloads/)

**液晶屏&驱动板**

这个比较幸运，从淘宝上买的N41屏幕，在另一家买的N41的驱动板，安装后一次点亮。比较坑的是当时没留意把驱动板放到铁器上面，驱动板电容烧毁了，又找卖家修了下重新配置。

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

**[树莓派仪盘表](http://maker.quwj.com/project/10)**

**[wifi自动连接配置](http://blog.csdn.net/geekcome/article/details/7325198)**


## 0x03 配置传感器

这边配置了`声音传感器`、`温湿度传感器`、`时钟`和`蜂鸣器`。代码都是从网上拷贝后修改的，要注意的是GPIO针脚。可以参考这篇[树莓派GPIO引脚对照表](http://shumeipai.nxez.com/raspberry-pi-pins-version-40)来配置。


## 0x04 搭建MagicMirror

**安装MagicMirror**

```bash
sudo apt-get install python-software-properties
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

#运行MagicMirrors
npm start
```




**supervisor管理脚本**
[配置参考](http://blog.csdn.net/xyang81/article/details/51555473)
```bash
#安装supervisor
pip install supervisor

#supervisor脚本

```


## 0xFF 参考资料

> [MagicMirror](https://github.com/MichMich/MagicMirror)
> [树莓派GPIO引脚对照表](http://shumeipai.nxez.com/raspberry-pi-pins-version-40)
> [配置wifi连接](http://blog.csdn.net/geekcome/article/details/7325198)