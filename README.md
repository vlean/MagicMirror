# MagicMirror
let's make a MagicMirror

## 0x00 准备

材料准备

- [x] 树莓派3b
- [ ] 温湿度传感器/人体感应器
- [ ] 声卡/3.5mm音箱/usb麦克风
- [x] 15.6 BOE N41屏幕/屏幕驱动板/hdmi线/电源
- [ ] 木制外框CAD制图/CAD加工定制


## 0x01 硬件篇

## 0x02 软件篇

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

```



**安装nginx/php服务**

**内网穿透服务**

**自动部署服务**

**supervisor管理脚本**





## 0xFF 参考资料

> [MagicMirror](https://github.com/MichMich/MagicMirror)
> 