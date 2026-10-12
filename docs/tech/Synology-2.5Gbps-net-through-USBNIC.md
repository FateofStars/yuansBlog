# 通过 USB 2.5G/5G/10G 网卡为群晖NAS提供高速网络连接

## 参考文档
[Youtube - 给Synology DS923+ NAS增加2.5G USB网卡的配置过程](https://www.youtube.com/watch?v=iu_Tv-68lng)

[Synology | 知识中心与文档库 - 群晖NAS 套件架构查询](https://kb.synology.cn/zh-cn/DSM/tutorial/What_kind_of_CPU_does_my_NAS_have)

[Github - DSM driver for realtek RTL8152/8153/8156/8157/8159 based USB Ethernet adapters](https://github.com/bb-qq/r8152?tab=readme-ov-file)

[什么值得买 - 群晖外接兮克2.5G网卡教程](https://post.smzdm.com/p/a4pn0kvw/)

## 文件下载

[网卡驱动套件 - relase page](https://github.com/bb-qq/r8152/releases)

## 使用工具
[putty](https://www.putty.org/?hl=zh-cn)

2.5Gbps USB有线网卡一个 (使用Realtek RTL8156解决方案)

!!! warning
    在购买网卡前，请确保网卡使用的方案为瑞昱的`RTL8152` `RTL8153` `RTL8156(1)` `RTL8157` `RTL8159`芯片
    { .annotate }

    1.   一般来说，市面流行的消费级2.5G有线网卡使用的是瑞昱的RTL8156方案


## 实机操作


!!! note 
    本次操作使用的机型为：Synology NAS DS923+ 

    系统版本为：DSM 7.2.1-69057 Update 8


### 查询NAS的CPU型号以及适用套件架构

通过[群晖NAS 套件架构查询](https://kb.synology.cn/zh-cn/DSM/tutorial/What_kind_of_CPU_does_my_NAS_have)，查询自己NAS所使用的套件架构，以DS923+为例，它在x23系列，在其子项目中可以找到下列表格 


型号|CPU 型号|核心数（单一 CPU）|线程数（单一 CPU）|FPU|<u>**套件架构**</u>|RAM
----|----|----|----|----|----|----
DS923+|AMD Ryzen R1600|2|4|✓|<u>**R1000**</u>|DDR4 ECC SODIMM 4 GB


我们需要注意的是<mark>**套件架构**</mark>一列，就比如这台机器的套件架构为R1000 

### 下载网卡驱动套件

!!! warning
    在购买网卡前，请确保网卡使用的方案为瑞昱的`RTL8152` `RTL8153` `RTL8156(1)` `RTL8157` `RTL8159`芯片
    { .annotate }

    1.   一般来说，市面流行的消费级2.5G有线网卡使用的是瑞昱的RTL8156方案


接下来，我们需要下载适用于我们机器的网卡驱动套件，首先，我们可以在购买网卡前向客服咨询网卡内使用的芯片，也可以根据[Github - DSM driver for realtek RTL8152/8153/8156/8157/8159 based USB Ethernet adapters](https://github.com/bb-qq/r8152?tab=readme-ov-file)的描述页面通过网卡的最大连接速率来推断网卡内使用的芯片。


然后进入[relase page](https://github.com/bb-qq/r8152/releases)，根据顶部的说明找到符合自己设备的下载链接下载驱动套件。

就比如我下载的就是`r8152-r1000-2.19.2-2_7.2.spk`


### 安装网卡驱动套件

!!! warning
    以下操作涉及SSH以及管理员权限的使用，请在使用时确保自己设备以及数据的安全，以免造成财产损失

让我们先进入群晖的网页，登录一个管理员账户，打开“**套件中心**”

接下来点击右上角的“**手动安装**”，选择并安装刚刚下载下来的网卡驱动套件

首次安装必定会提示

>无法正确安装此套件

<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/2.5Gnet/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202025-10-05%20111137.png" widgh=1000>

*（图片来自网络）*</p>
</p>
此时，我们需要前往“<strong>控制面板>终端机和SNMP</strong>”，勾选“<strong>启用SSH</strong>”功能，并保存设置


接下来，我们打开putty，输入NAS的IP地址以及SSH端口号，连接NAS的SSH


在SSH中登入管理员账号，输入<strong>`sudo install -m 4755 -o root -D /var/packages/r8152/target/r8152/spk_su /opt/sbin/spk_su`</strong>，并回车

这一操作并不会有任何提示，在看见
>用户名@设备名：~$

再次出现后，便可以关闭putty以及SSH功能

接下来，我们再次在套件中心手动安装套件，便可安装成功。

在插入网卡并启用套件后，我们便可以在“<strong>控制面板>网络>网络界面</strong>”看到“**局域网3**”

<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/2.5Gnet/QQ%E6%88%AA%E5%9B%BE20251005112654.png">

在将网卡连接网络后，能正常看见IP地址以及网络状态，网卡驱动便安装成功

!!! note ""
    在安装驱动后第一次重启可能会导致网卡套件停用，只需在套件中心再次启用便可

### 使用网络bond来增强网络连接的可用性

为了保障网络连接的稳定性与可用性，我建议再使用一条网线，将群晖自带的网口与另一台交换机连接起来，以提高网络连接的可用性，同时使NAS可以继续支持网络唤醒(1)
{ .annotate }

1.  <mark>**外置网卡不支持网络唤醒**</mark>


在将内置网卡接入交换机后，我们可以看见群晖获取到了两个IP地址

<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/2.5Gnet/QQ%E6%88%AA%E5%9B%BE20251005113356.png">

这时，我们需要将这两个网卡整合为一个bond

点击**新增**，选择**创建 Bond**，种类选择**自适应负载均衡**，选中已经连接上网络的两个接口

<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/2.5Gnet/QQ%E6%88%AA%E5%9B%BE20251005113657.png">

确认设置无误后，点击**下一步**，此时，NAS会提示

`在不同速度的接口上创建 bond 可能无法达到预期速度。是否继续？`

我们只需点击**是**，并配置好IP地址，即可点击**下一步**

此时，NAS会提示

`每个绑定接口上的服务(例如防火墙规则和 DHCP Server 设置)在 bond 创建之后会禁用。是否要继续？`

如果我们没有将这些服务绑定到选中的端口，我们只需点击**是**，即可让NAS自动创建Bond

待网页自动刷新后，我们便可以在网络界面中，看见“局域网1”和“局域网3”已经合并到了“Bond 1”中，并且总连接速率也达到了3500Mbps，至此，网络bond已经配置成功，我们可以继续使用NAS了

<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/2.5Gnet/QQ%E6%88%AA%E5%9B%BE20251005114553.png">

## 配置完成后的实际连接速率

在实际使用中，由于bond的原因，并不能真正将网速维持在2.5Gbps，通常在180MB/s到275MB/s周期性波动，但是即便如此网络速度也远超于千兆连接时的110MB/s

所以从总体来看，这次的升级十分成功

## Q&A

!!! question
    ### 为什么不使用矿神源提供的网卡驱动套件？

    - 在一开始，我是通过矿神的网卡驱动套件才意识到群晖NAS原来可以通过USB网卡提供网络</p>
    在实际操作中，一开始我也尝试通过这个方法来安装网卡驱动</p>
    然而，我发现似乎在DSM7.2中，群晖似乎无法通过这个套件来正常驱动网卡，并且使用这个套件的教程即使是我能找到的最新版本也是在半年前发布的了</p>
    所以，我找到了更新、通用性更强的教程</p>
    这个教程便是使用的blog中的方法来安装网卡驱动，在实际的尝试过后，发现可行，于是在blog上分享

    - 当然，如果你使用的是旧版本的DSM，不妨可以尝试一下以这种方式安装驱动，在操作上会比我使用的方便许多，具体教程可以参考[这个视频](https://www.bilibili.com/video/BV1WHUuY8EfF)
    


!!! question
    ### 为什么要放弃稳定的传输速度而选择使用Bond？

    - 最主要还是为了可用性，网络唤醒其实并不需要使用bond，使用bond对我来说最大的好处是可以将两个网卡统一为一个IP地址</p>
    由于我家中有设备是通过固定IP接入的NAS，同时还需要兼顾家里人通过IP访问NAS的可能性，于是统一的IP地址便成为了提高NAS可用性的刚需</p>
    - 将两个网卡创建为bond后，即使在USB网卡或者驱动发生故障时，也能够让其他内网设备继续通过原IP地址正常访问，不会导致连接中断，这便是我的考量

    ### 为什么使用“自适应均衡负载”而不是使用“活动/备用”模式呢？

    - 这是因为群晖的DSM的Link Aggregation设计问题，由于群晖的多个网卡均为同等速率，因此没有考虑(或许)到两个网卡不同速率的适用情形

    - 在使用活动/备用模式时，群晖会按照网卡的默认排序作为优先级顺序来调用网卡</p>
    即网口的优先级为`局域网1>局域网2>局域网3`</p>
    然而我们都知道，我们所外接的网卡为`局域网3`，`局域网1`对应的则是靠近电源接口的千兆网口</p>
    因此，若将Link Aggregation调整为活动/备用模式后，会出现虽然NAS显示连接速率为2500Mbps，但是实际传输速率只有千兆的情况
