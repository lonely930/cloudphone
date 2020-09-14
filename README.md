# cloudphone
2019.3.21

一时之间，云手机成为终端领域的热门产品和热点话题。不过，正如这突入起来的热潮，让许多第三方认识下意识敏感起来觉得又是一个圈钱的概念，2018年年底正式进入云手机市场，云手机概念有被大家再一次重新认识。

什么是X86架构？

言简意赅的说一下x86主要应用于PC领域笔记本、台式机、常见服务器-（Dell、IBM、HP）

什么是ARM架构？

ARM主要y应用于移动端 手机、平板、车载、穿戴设备

为什么需要用ARM架构来做云手机，由于Android操作系统是跑在arm环境下面的既然需要做手机那么就需要去模拟他的环境(既然说到模拟机环境大家都会联想到QEMU)。笔者可能也跟大多数人一样如何去实现这个云手机的应用，鉴于对虚拟化和云计算以及Java开发的一些工作经验初步有了一个 实现该项技术的方向。

首先借鉴，之前申请了云手机公测资格以及long境云，给到的测试环境为 VNC+ABD。通过VNC工具连接到远程Android端发现以下几个重点：

1、Android系统被重新打包阉割过，没有任何传感器，以及输出设备。通过各种硬件测评软件扫描结果 cpu 内存 硬盘都为虚拟的。

2、从cpu的标识看为rk3328的开发板，做过嵌入式开发的哥们应该很熟悉。->猜测通过qemu模拟3328开发板进行编译Android以及阉割。

3、通过购买选择数量就能立马出现新的Android云手机，这个特征完全符合虚拟机克隆。联想到vmware，xen，kvm等。

4、其中符合arm架构的虚拟化只有kvm，那么暂定kvm+qemu的组合来创建虚拟机并且模拟Android开发环境。

个人整理的实现思路如下：

1、云手机万物基于arm，购买开发板跑kvm+qmeu或者购买arm服务器。

2、安装linux系统以及kvm+qemu虚拟化，派生arm虚拟机。

3、arm虚拟机内搭建Android开发板模拟环境，编译调整Android驱动打包虚拟机并启动Android。

4、通过vnc远程控制接入arm虚拟机(也就是Android开发板模拟环境)。

5、整套流程跑通Java就该上场了，既然做云手机那么就需要增删改查自动化操作100台手机->云管平台开发！

6、整理libvirt api的官方文档，派生虚拟机，销毁、关闭、启动。等等虚拟机层面的操作

7、服务器内的一台台虚拟机需要通过vnc来控制以及传输画面，需要做图像压缩虚拟按键控制操作。

8、上云手机的目的就是为了批量，那么adb的端口以及脚本需要定制化一部分出来。喜欢玩大数据的公司或者机构一定需要xposed框架。

9、前端的UI界面后台的管理系统自行定义。

      提醒：市面上有一些手机云管平台 是基于x86架构做的arm指令集转换。并且Android系统用的是Androidx86，在性能上没法商用。(建议感兴趣的用物理arm环境)

商务咨询(私v)：matbls

