# 树莓派4B_linux clash部署教程（2024.3.4）

> 写在前面：由于clash去年已经删库跑路，加上网上对于这部分部署教程并不太多，前期个人也踩了很多坑，希望这篇对大家有帮助，enjoy~
# 0 使用情况介绍
* 硬件：树莓派4B
* 烧录系统 ："Bookworm", released on 11th October 2023
* Python 3.11.2
* VScode SSH-remote 连接 Raspberry Pi
# 1 下载Clash
## 1.1 为什么选择clash
之前在windows和Android设备都有使用过clash，感觉很流畅。其跨平台兼容性、用户友好的图形界面、灵活的配置选项都做得很好，并且支持多种代理协议，如Shadowsocks、Vmess等，可以能够根据规则智能地分流网络请求，优化网络访问速度和稳定性。此外，它还支持自动代理和直连规则，可以根据需要自定义，满足各类特定需求。
## 1.2 下载clash
首先需要了解清楚自己树莓派的操作系统位数。在树莓派命令行中输入
```
uname -m
```
根据不同的返回值判断系统位数和所需clash版本，如下表所示。
| 返回值 | 操作系统位数 | 所需下载的clash版本 |
| :--- | :---: | :---: |
| armv7l | 32位 | [clash-linux-armv7.gz](https://github.com/frainzy1477/clash_dev/releases/download/v1.1.0/clash-linux-armv7.gz) |
| aarch64 或 armv8 | 64位 | [clash-linux-armv8.gz](https://github.com/frainzy1477/clash_dev/releases/download/v1.1.0/clash-linux-armv8.gz) |

其他版本可通过[此链接](https://github.com/frainzy1477/clash_dev/releases)获取
![image](https://github.com/Xizhe-Hao/RaspberryPi-4B-clash-2024.3/assets/154408355/dd28b846-44aa-4ca7-8951-b79aee49bae4)
下载指令（以64位系统为例） 
```
wget https://github.com/frainzy1477/clash_dev/releases/download/v1.1.0/clash-linux-armv8.gz
```
解压下载文件  
```
gunzip clash-linux-armv8.gz   
```
将解压好的clashclash-linux-armv8文件更名为clash，移动到/usr/local/bin文件夹下，同时给予执行权限
```
mv clash-linux-armv8 clash
sudo mv clash /usr/local/bin
sudo chmod a+x /usr/local/bin/clash
```
# 2 配置clash
## 2.1 下载clash配置文件
```
wget -O config.yaml [订阅链接]
```
订阅链接请从魔法后台寻找
![image](https://github.com/Xizhe-Hao/RaspberryPi-4B-clash-2024.3/assets/154408355/c6209992-8ddd-42f3-b287-718d4e9604d9)
## 2.2 下载clash全球IP库
Clash配置需要下载Country.mmdb 文件，Country.mmdb 是全球 IP 库，可以实现各个国家的IP信息解析和地理定位，没有这个文件clash无法正常启动下载指令
```
wget -O Country.mmdb "https://raw.githubusercontent.com/SukkaW/Koolshare-Clash/master/koolclash/koolclash/config/Country.mmdb"
```
## 2.3 放置配置文件
在.config文件夹下新建clash文件夹
```
cd .config
mkdir clash
```
将上述配置文件放置在.config/clash文件夹下面
```
mv config.yaml Country.mmdb clash/
```
# 3. 环境配置
回到主目录，通过对bashrc中文件进行修改来配置clash环境
```
cd
sudo nano .bashrc
```
在打开的bashrc文件最下方添加环境配置指令
```
export http_proxy="http://127.0.0.1:7890"
export https_proxy="http://127.0.0.1:7890"
export all_proxy="socks5://127.0.0.1:7891"
```
使环境指令生效
```
source .bashrc
```
重启树莓派，使配置生效。
```
sudo reboot
```
# 4 测试
重新开机后输入clash启动指令，在~/.config/clash目录下查找其配置文件
```
clash -d ~/.config/clash
```
如果返回类似下方内容证明成功启动
```
INFO[0000] Start initial compatible provider 💬chatGPT   
INFO[0000] Start initial compatible provider 🎬Netflix   
INFO[0000] Start initial compatible provider 🎵Spotify   
INFO[0000] Start initial compatible provider 🎬Youtube   
INFO[0000] Start initial compatible provider 🔰国外流量      
INFO[0000] Start initial compatible provider ⚓️其他流量     
INFO[0000] Start initial compatible provider ✈️Telegram 
INFO[0000] Start initial compatible provider 🎬国外媒体      
INFO[0000] Start initial compatible provider 🎬哔哩哔哩      
INFO[0000] Start initial compatible provider 🍎苹果服务      
INFO[0000] Start initial compatible provider 🎬Disney+   
INFO[0000] Start initial compatible provider 🚀直接连接
```
