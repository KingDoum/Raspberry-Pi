# Raspberry-Pi
树莓派资源整理汇总及分享

> 防背刺声明：'
> 
> 本仓库克隆于 xinxingli/Raspberry-Pi:master
> 但是很多连接已经失效，因此重新进行数据整理，方便用户直接按图找数据
> 
> 二次整理日期：2023.09.17




本文收集了树莓派使用过程中经常需要用到的资源，主要包括树莓派系统镜像、树莓派硬件介绍、树莓派GPIO引脚编号、树莓派电路原理图下载、树莓派应用等等，非常值得收藏。

## 一、快速上手
所有资源均来自此网址：

[树莓派快速开机资源大全](https://shumeipai.nxez.com/download) ：包括**树莓派快速开机指南 、系统镜像下载、烧录软件，帮助您快速上手。**

## 二、系统镜像

[树莓派（raspberrypi）常用镜像高速下载](https://make.quwj.com/member/2/bookmarks?category=37) ：收集了超过12种树莓派系统镜像，同时带有介绍，你可以选择一个最佳的树莓派系统，在页面即可下载系统镜像，非常方便。

## 三、硬件介绍

[树莓派开箱-上手简评](https://shumeipai.nxez.com/hot-explorer#beginner)

GPIO编号：[树莓派GPIO引脚对照表](https://shumeipai.nxez.com/raspberry-pi-pins-version-40)

## 四、树莓派配置

树莓派需要烧录一个系统，这样才可以运作，具体教程参考
https://zhuanlan.zhihu.com/p/594475040

**建议直接装64位版本，用起来性能会稍微好点；
Python 也别装包了，直接上mimnconda 方便管理**
PS 需要看一下 CPU

> 命令查看
在终端运行以下命令可以查看SoC型号:
> copy code
> 
> cat /proc/cpuinfo
> 
> 如果 processor 字段显示 BCM2711,则是四核版本;如果显示 BCM2711A,则是八核版本。
> 
> 四核装32位，八核装64位






里面有软件烧录和wifi配置。启动后，可以直接连上家里的wifi。在路由器上找到
这个IP地址，然后直接ssh连接配置即可。

连接后，按照上面说的，更新数据源配置，更新一下数据




## Pycharm 远程部署 代码

建议按照这上面的行为进行连接电脑
https://zhuanlan.zhihu.com/p/267836740

设置几个软件

## 五、树莓派软件源 & 镜像源

另外更新软件的时候，要选择架构对应的源
http://btharp.com/article/2022/6/24/107.html

### 软件源更新

可以参考此文档:[参考文档](https://zhuanlan.zhihu.com/p/98079246)


#### 1、国内常用软件源


sudo gpg --keyserver keyserver.ubuntu.com --recv-keys 9165938D90FDDD2E
gpg --export --armor 9165938D90FDDD2E | sudo apt-key add -
使用lsb_codename -a 可以查询版本，来选择需要种源

[树莓派系统常用中文镜像源](http://mirrors.tuna.tsinghua.edu.cn)

#### 2、树莓派镜像源列表

[www.raspbian.org/RaspbianMirrors](http://www.raspbian.org/RaspbianMirrors)

## 六、树莓派应用

仅整理自己想要尝试的项目，更多项目，在此链接中寻找

[树莓派可以用来什么？](https://shumeipai.nxez.com/what-raspi-used-for)

个人尝试的项目

1. [Pi Dashboard (Pi 仪表盘)
设备监控 仪表盘面板](https://make.quwj.com/project/10)
2. 在树莓派闲置的时候，上传PCDN 利用网心云赚点钱 [具体链接](https://help.onethingcloud.com/7cb4/3ed5/39ae) (PS：如果链接挂了，可以直接点击此 跳转到[首页](https://help.onethingcloud.com/7cb4/3ed5)，待补充) 

## 七、树莓派相关手册
在此处阅读
[raspberry-pi-beginners-guide-zh-cn-v1.1.pdf](raspberry-pi-beginners-guide-zh-cn-v1.1.pdf)

## 八、树莓派版本代码大全
Raspberry Pi 树莓派已经发布了很多个版本。每一版树莓派都有唯一的版本代号，通过下面这行命令可以查看这个代号：

cat /proc/cpuinfo

最后三行表示主板的硬件型号、版本代号和唯一的序列号

[有兴趣请点击此链接了解详情](https://shumeipai.nxez.com/raspberry-pi-revision-codes)





