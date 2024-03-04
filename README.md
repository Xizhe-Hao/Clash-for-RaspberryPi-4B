# 树莓派4B_linux clash部署教程（2024.3.4）

> 写在前面：由于clash去年已经删库跑路，加上网上对于这部分部署教程并不太多，前期个人也踩了很多坑，希望这篇对大家有帮助，enjoy~
# 0 使用情况介绍
* 硬件：树莓派4B
* 烧录系统 ："Bookworm", released on 11th October 2023
* Python 3.11.2
* VScode SSH-remote 连接 Raspberry Pi
# 1 Clash
## 1.1 为什么选择clash
之前在windows和Android设备都有使用过clash，感觉很流畅。其跨平台兼容性、用户友好的图形界面、灵活的配置选项都做得很好，并且支持多种代理协议，如Shadowsocks、Vmess等，可以能够根据规则智能地分流网络请求，优化网络访问速度和稳定性。此外，它还支持自动代理和直连规则，可以根据需要自定义，满足各类特定需求。
## 1.2 下载clash
首先需要了解清楚自己树莓派的操作系统位数。在树莓派命令行中输入'''uname -m'''，如果返回"armv7l" ，即为32位操作系统；如果返回"aarch64" 或 "armv8"即为64位操作系统。  
接下来根据对应的操作系统下载不同版本的clash，树莓派 4B 32 位操作系统 ，那么你应该下载对应 armv7 版本的 clash-linux-armv7-v1.11.0.gz，如果是 树莓派 4B 64 位操作系统 ，那么你应该下载对应 armv7 版本的 clash-linux-armv8-v1.11.0.gz我个人选择在主机上
![image](https://github.com/Xizhe-Hao/RaspberryPi-4B-clash-2024.3/assets/154408355/dd28b846-44aa-4ca7-8951-b79aee49bae4)

