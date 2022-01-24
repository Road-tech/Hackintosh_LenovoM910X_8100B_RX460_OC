# Hackintosh for Lenovo-M910X, I3-8100B, RX460, using Opencore and Support to macOS Monterey

**使用EFI前请务必修改三码(SSN,UUID,ROM)**    
**Please change three system codes (SSN,UUID,ROM) before using this EFI**   

---

2021-01-21 更新

- Update Opencore to 0.7.7
- Update Lilu to 1.5.9
- Update VirtualSMC.kext to 1.2.8
- Update AppleALC.kext t0 1.6.8
- Update IntelMausi to 1.0.7
- Update SMCProcessor to 1.2.8
- Update SMCSuperlO to 1.2.8
- Update WhateverGreen t0 1.5.6 
- Support to macOS Monterey 12.1

---

# 关于M910X



![](https://raw.githubusercontent.com/Road-tech/Road-blog-Figure/main/2022/01/24-10-58-47-lenovo-thinkCentre-M910x-tiny-hero-front-1.jpg)

图源联想官网(https://www.lenovo.com/us/en/p/desktops/thinkcentre/m-series-tiny/m910x-tiny/11tc1mt910x)[https://www.lenovo.com/us/en/p/desktops/thinkcentre/m-series-tiny/m910x-tiny/11tc1mt910x]



联想的M910X（p320 tiny），一个非常好玩的1L迷你小主机。Q270的主板，双M.2插槽、一个PCIe扩展槽、双通道ddr4、6个USB，同时是最后一代可以刷bios上魔改U的联想小主机。再往上的M920x，P340都是双BIOS设计，无法刷bios了，也基本告别了便宜好玩的ES版CPU或者魔改U。

现在这台小主机性价比非常高，700出头的价格就买到这样的强悍扩展性，放在这个价位简直无敌的存在，而且可玩性非常高。这个价格换成300系芯片组的小主机，基本都没有双M.2接口（除了dell 7080mff 低压版），更别说PCIe扩展了。而他的下一代M920X，现在还要1300的价格，相比起来只多了个typc-C接口，不过原生可以上8代U，但只能支持正式版。M910x原配的显卡为RX460，现在咸鱼原厂全新只要600左右的价格。而M920x配套的rx560现在咸鱼要差不多1000，一个性能差不多的马甲卡居然贵那么多。当然如果不追求黑苹果，只为最强的独显性能，最新的M930X，原厂可以选配到P620。当然动手能力强的可以上GTX1650，妥妥的小钢炮，就是要切挡板，考验手艺。

## 我的硬件配置

|                     | Specifications / 型号               | Note / 备注 |
| ------------------- |:---------------------------------:|:---------:|
| Motherboard/主板:     | Lenovo M910X Q270                 | 1L 迷你主机   |
| CPU/处理器:            | I3-8100B                          | 闪电家魔改U    |
| CPU Cooler/散热器:     | 准系统自带                             |           |
| Hard Drive/硬盘:      | Hikvision C2000pro 512gb          |           |
| RAM/内存:             | Samsung 8G DDR4 2666MHz X1        |           |
| Wireless Card/无线网卡: | BCM94360cs2+转接卡                   | 白果拆机卡     |
| Tower Case/机箱:      | 准系统自带                             |           |
| Power/电源:           | 55/25mm 19v 120w DC power adapter |           |

---

# 一些折腾点

## 关于魔改U

聊回这台M910X，要上这个魔改U8100B/8500B/8700B，需要刷入修好的BIOS。一般魔改u的老板都会提供一个修好的BIOS，而闪电家给我提供的bios，虽然能点亮，但是因为没有写入S/N等信息，开机滴滴滴两声报错，而且BIOS版本也太老了，还关不掉cfg-lock。

于是需要自己修改bios，如果你有6代或者7代的亮机U，这是最方便的，直接进windows更新bios到最新版本。这里感叹一下，这台2018年就发布的机器，到2021-7-6居然还更新了一版BIOS，感觉换成那些零售的diy主板，早就停止支持了。

官方BIOS下载地址在这里：[点我下载](https://newsupport.lenovo.com.cn/driveList.html?fromsource=driveList&selname=ThinkCentre%20M910x)

更新完BIOS后，用编程器把BIOS提取出来，使用D大的工具进行魔改，具体操作请参考：[部分 Lenovo 联想 LGA1151 主机 支持 8 代 9 代 BIOS 修改工具](http://www.smxdiy.com/thread-1299-1-1.html)

如果没有亮机U，那只能用编程器直接把BIOS提取出来，参照上面链接里的强刷教程，刷入魔改bios后，进Windows用WriteSN工具补回S/N等信息。

闪电家提供的BIOS和自己修改的BIOS我都放在了[魔改BIOS](https://github.com/Road-tech/Hackintosh_LenovoM910X_8100B_RX460_OC/tree/main/魔改BIOS)的文件夹内，可自行下载研究。

***如果你选择直接刷入这两版BIOS，而不是自己提取修改，请务必用WriteSN工具补回原机的S/N等信息***

BIOS芯片为25L12873F，具体位置参考这个图（图源自SMZDM的[折了个腾](https://zhiyou.smzdm.com/member/9509386572/)）

![BIOS芯片](https://raw.githubusercontent.com/Road-tech/Road-blog-Figure/main/2022/01/24-00-47-21-5ecdd94a3f0322866.jpg_e1080.jpg)

![具体位置](https://raw.githubusercontent.com/Road-tech/Road-blog-Figure/main/2022/01/24-00-47-11-5ecdd94a44bfd6611.jpg_e1080.jpg)

## 关于CFG-Lock

一般来说，黑苹果想要实现完美的休眠，关闭CFG-Lock是必要条件的。

如果你选择刷入我自己改好的BIOS，那这个BIOS已经把隐藏的CFG-lock开关显示出来，直接在BIOS里面关闭就好了。

如果选择自己修改BIOS，BIOS没有CFG-lock选项，可以用opencore引导解锁。在启动菜单选择页面选择ControlMsrE2，我已经在EFI默认配置了unlock参数，进入后可以看到CFG-lock的状态，同时尝试解锁CFG-lock。

## 关于显卡

这台小主机有两张显卡，分别是CPU的UHD630以及独显RX460。

在macOS里，即便有独显，核显还是有作用的，可以用于加速，所以630核显直接配置```AAPL,ig-platform-id```为```0300913E ```，不需要做更多的显卡输出修复。

独显直接免驱动，Emmm，这算是我折腾过最简单的方案了。

## 关于声卡仿冒

省流助手：```layout-id```为```12```，也就是```0C000000```

一开始我参照了网上现有的opencore配置，发现声卡仿冒的```layout-id```一般都是设置为11和21两种。我分别试了下，设置11的时候主机的内置音箱有声音，插耳机没声音。设置为21的时候情况相反。不完美很难受

后来网上查资料看到这种情况，需要自己定制仿冒声卡，于是我参考了[OpenCore引导安装联想-M920x黑苹果之历程](https://shuiyunxc.gitee.io/2020/03/21/M920x/index/)这篇文章，按照文章给出的参数自己编译了AppleALC.Kext。但是怎么弄都不行，明明所有参数都是对的，最后才发现原来这是M920X教程，汗- 。-！。（M910X的兄弟型号是P320 tiny，导致我老是把M910X记成M920X）

M920X的声卡是ALC235，而M910X的声卡是ALC294，也就是这些参数并不通用。自己仿冒声卡步骤超级无敌复杂，无敌头疼。但是在GitHub翻AppleALC的代码的时候，发现2018年7月的时候MacPeet提交了关于Realtek ALC294 for Lenovo M710Q的仿冒配置，```layout-id```为12。考虑到同一代的小主机的硬件设计高度相同，于是就去试了下12，果然是完美的！内置音箱和耳机都正常工作。所以```layout-id```设置为12就好了！感谢MacPeet大佬。

## 关于网卡的选择

黑苹果的网卡选择有很多，图简单省事的话，可以选黑果小兵的BCM94360Z3或者BCM94360Z4。应该加个kext就可以完美驱动了。
链接可以参考这[【黑果小兵独家】BCM94360Z4/BCM94360Z3 m.2 NGFF接口四天线笔记本/小主机专用黑苹果无线网卡驱动教程](https://blog.daliansky.net/BCM94360Z4-m.2-NGFF-interface-four-antenna-notebook_small-host-dedicated-black-Apple-wireless-network-card-driver-tutorial.html)

不过考虑到m910x内部对无线网卡的高度没什么限制，最推荐的还是苹果iMac拆机的BCM94360cs2或者BCM943602cs配合转接卡，什么驱动都不用补，最省事。但是长度有限制，BCM94360cs2要磨掉一点PCB才能刚刚好放进去，更长的BCM943602cs就别想了。所以这里推荐反向的转接卡，再用点热熔胶固定。

具体可以参考这个图：（图源自SMZDM的[折了个腾](https://zhiyou.smzdm.com/member/9509386572/)）

![反向转接卡](https://raw.githubusercontent.com/Road-tech/Road-blog-Figure/main/2022/01/24-00-47-41-5ecdd94fe51db2827.jpg_e1080.jpg)

## 关于BIOS设定

### Disable：

- 设备：

- System Agent(SA) Configuration -> VT-d 

- ATA设备清单 -> Configure SATA as -> AHCI  

- 显示菜单 -> Auto

- 网络菜单 -> PXE启动 

- 高级菜单：

- CPU Configuration -> SW Guard Extensions (SGX)

- Power & Performance -> CPU -> CPU Lock Configuration -> CFG Lock  

- 启动菜单：

- 兼容模块

### Enable：

- 设备：

- System Agent(SA) Configuration -> Above 4G MMIO BIOS assignment  

- 高级菜单： 

- CPU Configuration -> Intel (VMX) Virtualization Technology (VT-x)  

- 启动菜单：

- 启动方式：UEFI  

---

# Functions/功能

### Work：

- All DP ports (1080p) on RX460  
- Audio output on DP  
- All USB ports  
- Wi-Fi & Bluetooth  
- 3.5mm Audio Jack and Internal Mic
- Airdrop  
- AirPlay  
- Continuity  
- QE/CI of Intel UHD 630 & rx460
- CPU Power Management
- Sleep 

### Not working:

- - 

### Not tested yet:

- 4k display  

---

# Performance/展示

我已经超级无敌懒，根本不想自己拍照，都是网上现找的图，如侵删。

以下图源自[English Community-Lenovo Community](https://forums.lenovo.com/t5/ThinkCentre-A-E-M-S-Series/Lenovo-M910x-Tiny-Extreme-RX-460-graphics-option-details-and-avail/m-p/3725943?page=2)以及[联想官网](https://www.lenovo.com/us/en/p/desktops/thinkcentre/m-series-tiny/m910x-tiny/11tc1mt910x)

![](https://raw.githubusercontent.com/Road-tech/Road-blog-Figure/main/2022/01/24-11-10-50-lenovo-thinkCentre-M910x-tiny-hero.png)

![](https://raw.githubusercontent.com/Road-tech/Road-blog-Figure/main/2022/01/24-11-12-54-lenovo-thinkCentre-M910x-tiny-mdp-ports-5.png)

![](https://raw.githubusercontent.com/Road-tech/Road-blog-Figure/main/2022/01/24-11-12-24-lenovo-thinkCentre-M910x-tiny-left-right-side-7.png)

![](https://raw.githubusercontent.com/Road-tech/Road-blog-Figure/main/2022/01/24-11-13-35-121854iF16DE5EBBC821E4B.png)

![](https://raw.githubusercontent.com/Road-tech/Road-blog-Figure/main/2022/01/24-11-13-41-121853iE5F561AB0C4920BB.png)

![CPU变频&显卡驱动正常](https://raw.githubusercontent.com/Road-tech/Road-blog-Figure/main/2022/01/24-10-33-45-CPU%26%E6%98%BE%E5%8D%A1.png)

![USB定制](https://raw.githubusercontent.com/Road-tech/Road-blog-Figure/main/2022/01/24-10-33-54-USB%E5%AE%9A%E5%88%B6.png)

![蓝牙工作正常](https://raw.githubusercontent.com/Road-tech/Road-blog-Figure/main/2022/01/24-10-34-02-%E8%93%9D%E7%89%99.png)

![Wi-Fi工作正常](https://raw.githubusercontent.com/Road-tech/Road-blog-Figure/main/2022/01/24-10-34-12-Wi-Fi.png)

---

# Reference/参考

[Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)

[精解OpenCore](https://blog.daliansky.net/OpenCore-BootLoader.html) - [黑果小兵的部落阁 ](https://blog.daliansky.net/)

[使用OpenCore引导黑苹果](https://blog.xjn819.com/?p=543) - [XJN](https://blog.xjn819.com/) 

[acidanthera/AppleALC](https://github.com/acidanthera/AppleALC)

[xia54/Hackintosh-Lenovo-Thinkcentre-M910x-OpenCore-Efi](https://github.com/ylen0l/Hackintosh-Lenovo-Thinkcentre-M910x-OpenCore-Efi)

[[SUCCESS] Lenovo M720q , M920q, P330 Tiny Catalina 10.15.6 OPENCORE](https://osxlatitude.com/forums/topic/13992-success-lenovo-m720q-m920q-p330-tiny-catalina-10156-opencore/)

[chencaidy/Hackintosh-OC-Lenovo-ThinkCentre-M920x](https://github.com/chencaidy/Hackintosh-OC-Lenovo-ThinkCentre-M920x)

[一台比较完美的黑苹果小主机 联想M910Q折腾记 opencore EFI分享](https://post.smzdm.com/p/ammkv6vv/)
