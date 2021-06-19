---
layout: post
title: Lenovo_Tab4_8_REL非常状态修复办法
tags: 嵌入式系统
---

<center><h3>Lenovo Tab4 8 REL 非常状态修复办法</h3></center><br />
<center><h4>二〇二〇年七月初一 水曜日</h4></center><br />
<center>***WARNING***</center><br />
　　本仓库及文章仅以研究性学习记录为性质，以开拓思路与研究计算机原理为目的；如任何人因模仿、学习、实践此仓库或文章内容造成的损失，后果自行承担。<br />
　　另，本仓库及文章所用到的软件、操作系统、应用程式等的知识产权归属其来源人或公司（高通无线通信技术有限公司、联想集团、Google Inc.、Android Open Source Project）。<br />
　　另，本文章所涉及的操作方法与步骤完全由个人研究所得，此方法步骤根据知识共享署名-相同方式共享 4.0 国际许可协议完全开源（与公有）。<br />

<h3><center>序</center><br /></h3>

　　**本人坚决贯彻GNU自由软件运动精神，本人的目标同[GNU](https://www.gnu.org/)及[FSF](https://www.fsf.org/)一样，“我们的目标是创建一个用户能在任何计算机上使用自由软件完成一切的世界，一个绝无第二人而是用户自己完全掌控其个人电脑的世界。”<br/>
　　鉴于此，私以为，在用户拥有计算机物权的前提下，在不侵犯他人合法权益的前提下，用户应能够自由、自主地使用、管理、支配计算机的任何部分；能够自由、自主地定制、二次开发自己的设备，而不应受到限制。既如此，一定的对设备在非正常状态下的修复能力与紧急处理能力是必要的。但现阶段由于各对市场占有、利润盈收、知识产权等保护的要求，以致设备自身封闭、技术资料少之又少，实是与广大研究者、劳动者之意志相悖。故对本课题进行研究，并完全开放研究成果，以便于诸位对计算机的研究与在操作失误等紧急状况下进行的修复与纠正；为自由软件运动尽一份力，为计算机界的发展进步尽一份力。<br/>
　　但注意，在诸位实践此项目时，须保证您拥有设备的物权，或得到物权所有者的许可，且不可以此损害他人合法权益，否则造成的任何损失与影响本人概不负责。且您对此项目的实践与否本人概不干涉，完全由您自己决定，与本人无关。**<br/>

<h3>〇、背景</h3>
----

　　近日由于特别原因，需要恢复一台Lenovo Tab 4 8 REL（SoC为高通APQ8017），而因无法以正常方式引导进入fastboot或EDL，无法正常恢复。据研究，本设备的bootloader与处于非常状态，无法根据用户操作进入recovery、fastboot或EDL，故另辟蹊径，寻找新的修复方案。<br />

<h3>准备</h3>
* 固件、驱动程式等，见[Lenovo_8X04F_Reflashing仓库](https://github.com/JunASAKA/Lenovo_8X04F_Reflashing)
* 十字1.2螺丝刀
* 镊子（导电；用于短接，可替换）
* Windows x86或x64操作系统（本次以Windows 10 LTSC 2019 x64演示）
* 经试验对于Linux平台，有更灵活的解决方案。详情请看[Android-OperSys-Kits仓库](https://github.com/JunASAKA/Android-OperSys-Kits/)。
<br/>
  
<h3>一、技术信息</h3>
----
![msm8917电路布局](https:\/\/cdn.jsdelivr.net/gh/1st-Science-Academy-of-E-S-C/1st-Science-Academy-of-E-S-C.github.io/assets/2020-08-19-Lenovo_Tab4_8_REL非常状态修复/msm8917电路布局.jpeg)<br />
　　图为msm8917（与设备所用的APQ8017相似，可作为参考）仅有的资料。一般来说，Android操作系统的启动流程如下图所示。在上电之后会进入bootloader，由bootloader根据用户的需要引导进不同的模式。高通处理器的bootloader为LittleKernel。而在上文提到的bootloader损坏或在bootloader由于种々原因无法引导进入其它模式的情况下，只能设法绕开bootloader以进行修复。但高通针对如上之情况，有相应的解决方案，即通过硬件（开关）来引导进入EDL紧急恢复模式（大致可根据电路图找到相应的触点，通过短接进入EDL）。<br />
![Android启动流程](https:\/\/cdn.jsdelivr.net/gh/1st-Science-Academy-of-E-S-C/1st-Science-Academy-of-E-S-C.github.io/assets/2020-08-19-Lenovo_Tab4_8_REL非常状态修复/Android启动流程.png)
<h3>二、软件准备</h3>
----
　　本次演示所用操作系统为Windows 10 LTSC 2019 x64，不同的操作系统在驱动程式的安装过程中可能有所不同，请注意根据实际情况进行操作。<br />
　　首先安装EDL模式的驱动程式。一般来说，Windows系统对驱动程式都会有签名检查，而高通所提供的驱动程式没有符合要求的签名。故要使用禁用驱动程式签名强制，在执行安装过程。对于几乎所有的Windows系统，在启动选项中都会有“禁用驱动程式签名强制”模式；对于Windows 10，有相对来说更简单的解决方案，即测试模式。<br />
　　进入方法如下。以管理员身份运行PowerShell或cmd，键入以下命令：bcdedit /set testsigning on（注意使用半角字符），然后按回车键。不出意外，应该如下图所示；此时再启动计算机，桌面右下角应会显示“测试模式”相关字样，则表明进入成功。在一切操作结束后，可以选择关闭测试模式，使用命令bcdedit /set testsigning off并再启动。<br />
![测试模式](https:\/\/cdn.jsdelivr.net/gh/1st-Science-Academy-of-E-S-C/1st-Science-Academy-of-E-S-C.github.io/assets/2020-08-19-Lenovo_Tab4_8_REL非常状态修复/测试模式.png)<br />
　　进入测试模式或“禁用驱动程式签名强制”模式后，可以开始安装驱动程式。解压缩Git仓库中的驱动程式，运行其中的Setup.exe，按照其步骤安装即可（请注意安装过程中的许可条款，直接忽略掉可能会引发种々麻烦）。<br />
　　接下来安装QPST的主程式。QPST为高通公司开发的，适应于高通芯片的实现烧录与调试等功能的程式集，本次仅々使用其烧录这一功能。该程式的安装程式可Git仓库中找到，在Windows系统下正常地安装即可。<br />
![QPST安装](https:\/\/cdn.jsdelivr.net/gh/1st-Science-Academy-of-E-S-C/1st-Science-Academy-of-E-S-C.github.io/assets/2020-08-19-Lenovo_Tab4_8_REL非常状态修复/QPST安装.PNG)
<br />
　　安装过后，运行程式集中的QFIL，在“Select Build Type”下选择“flat Build”；点击“Select Programmer”右侧“Browse…”按键，定位到固件所在目录，选择对应文件；再点击下方“Select Build”旁“Load XML…”按键，选择固件中的相应文件，这之后应会弹出另一个资源管理器窗口，在此选择固件中patch0.xml。之后应如图所示。<br />
![固件准备](https:\/\/cdn.jsdelivr.net/gh/1st-Science-Academy-of-E-S-C/1st-Science-Academy-of-E-S-C.github.io/assets/2020-08-19-Lenovo_Tab4_8_REL非常状态修复/固件准备.png)

<h3>三、硬件准备</h3>
----
　　接下来要对硬件进行操作，请万分小心；在此之前，最好了解一下电学的基础知识，以免误操作引发不必要的麻烦。在此之前请将设备完全关机。<br />
![撬开后盖](https:\/\/cdn.jsdelivr.net/gh/1st-Science-Academy-of-E-S-C/1st-Science-Academy-of-E-S-C.github.io/assets/2020-08-19-Lenovo_Tab4_8_REL非常状态修复/撬开后盖.PNG)
![螺丝](https:\/\/cdn.jsdelivr.net/gh/1st-Science-Academy-of-E-S-C/1st-Science-Academy-of-E-S-C.github.io/assets/2020-08-19-Lenovo_Tab4_8_REL非常状态修复/螺丝.PNG)<br />
　　首先借用IFixIt的两张图片，如上图撬开后盖（从micro USB接口或micro SD卡槽出入手比较方便），之后使用十字螺丝刀拧下八颗螺丝，揭下金属面板，则可以看到主板。如下图所示。<br />
![主板](https:\/\/cdn.jsdelivr.net/gh/1st-Science-Academy-of-E-S-C/1st-Science-Academy-of-E-S-C.github.io/assets/2020-08-19-Lenovo_Tab4_8_REL非常状态修复/主板.jpeg)<br />
　　这是近距离图片。<br />
![触点附近](https:\/\/cdn.jsdelivr.net/gh/1st-Science-Academy-of-E-S-C/1st-Science-Academy-of-E-S-C.github.io/assets/2020-08-19-Lenovo_Tab4_8_REL非常状态修复/触点附近.jpeg)<br />
　　现在需要找到电路板上保留的用于短接的触点，经试验，触点所在处如下。<br />
![触点](https:\/\/cdn.jsdelivr.net/gh/1st-Science-Academy-of-E-S-C/1st-Science-Academy-of-E-S-C.github.io/assets/2020-08-19-Lenovo_Tab4_8_REL非常状态修复/触点.jpeg)<br />
　　如图，用镊子或其它导体短接该二触点；与此同时将设备通过USB接入计算机。<br />
![短接](https:\/\/cdn.jsdelivr.net/gh/1st-Science-Academy-of-E-S-C/1st-Science-Academy-of-E-S-C.github.io/assets/2020-08-19-Lenovo_Tab4_8_REL非常状态修复/短接.jpeg)<br />
　　接入后，松开镊子或其它导体；此时设备的充电指示灯应为红色常亮状态，除此以外没有任何反应，若进入失败，则将设备关机后重试。若成功，在QFIL处应显示“Qualcomm HS-USB QDLoader 9008”；或“Select Existing Ports…”，这种情况下点击右侧“Select Port…”并选择相应端口。<br />
　　硬件准备完成。<br />

<h3>四、烧录</h3>
----
　　点击蓝色Download按键开始烧录，此过程经试验大概需要五分钟，设备会被完全清空并重新烧写，请注意备份数据（if possible）。之后下方蓝色进度条会出现反应。<br />
　　若如下图所示，日志框中显示蓝色的“Download Succeed”与“Finish Download”则表示烧写成功。<br />
![烧录完成](https:\/\/cdn.jsdelivr.net/gh/1st-Science-Academy-of-E-S-C/1st-Science-Academy-of-E-S-C.github.io/assets/2020-08-19-Lenovo_Tab4_8_REL非常状态修复/烧录完成.png)<br />
　　由于各计算机的操作系统版本不同、环境不同等原因，原文所述安装驱动程式的方式可能不适用于所有计算机。比如出现“安全策略不允许”、烧录时卡在一开始等情况，这些问题有一个通用的解决方案，即卸载已安装的出现问题的Qualcomm QDLoader 9008驱动程式（如果先前安装过而不好用），使用Windows系统的“禁用驱动程式签名强制”模式再次安装，不出意外，这样就能够正常运作。<br />
　　成功后，断开设备与计算机的连接，长按电源键一段时间，直到其再启动；烧录过程正式结束。<br />
　　第一次启动时可能耗时相对较长，并且会出现如下画面，表示正在准备userdata分区。<br />
![首次开机](https:\/\/cdn.jsdelivr.net/gh/1st-Science-Academy-of-E-S-C/1st-Science-Academy-of-E-S-C.github.io/assets/2020-08-19-Lenovo_Tab4_8_REL非常状态修复/首次开机.jpeg)<br />
　　启动如图。<br />
![完成](https:\/\/cdn.jsdelivr.net/gh/1st-Science-Academy-of-E-S-C/1st-Science-Academy-of-E-S-C.github.io/assets/2020-08-19-Lenovo_Tab4_8_REL非常状态修复/完成.jpeg)<br />
<h3>五、观念提升</h3>
----
　　不仅仅是这一型号的设备，几乎所有的高通处理器设备都可以通过如上方法烧写操作系统，在网路上俗称为“9008刷机”。如果相应的高通设备能够找到符合的固件，都可以通过短接主板上某些触点（或让某个或某些触点接地，即下拉到零电位点）进入EDL模式，执行烧录；但具体的短接点需要经过试验查找。<br />
　　本次研究对于非正常状态下设备的修复提供了一种可行的解决方案。<br />
  

