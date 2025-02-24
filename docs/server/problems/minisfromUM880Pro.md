# 在使用铭凡UM880Pro时注意到的问题

不得不说，AMD在小主机上确实是一个好选择，可以在较低的价格购买到较强的性能

我当初选择这台UM880Pro就是因为其有极强的核显性能，能够很好地胜任视频的转码推流工作，同时带有两个2.5G网口和一个OcuLink接口，拓展性可以说时极强的

然而，即使这样，这台机器的问题还是不小

## 苏妈的背叛

之前在高考完选电脑的时候，就知道AMD会出现兼容性问题，所以加钱上了intel

不过由于这台小主机用的是APU，所以最令人难受的兼容性问题也是大摇大摆地来了

其中影响最明显的就是Jellyfin，我当初选择这台小主机就是用来做转码工作的，不得不说，Radeon 780M核显性能也是十分强劲

但是，强劲又有什么用？

只要你使用转码功能，大概率就能在日志里看见

```
[AVHWDeviceContext @ 000001659f713f40] Using device 1002:1900 (AMD Radeon 780M Graphics).
[AVHWDeviceContext @ 000001659f714440] No matching devices found.Device creation failed: -19.
Failed to set value 'opencl=ocl@dx11' for option 'init_hw_device': No such device
Error parsing global options: No such device
```

**No such device**这句话简直成为了我的梦魇，每次只要Jellyfin出现问题，一打开日志就能看见这句话

无论是更新、重装驱动，还是外接HDMI诱骗器

我们试过了所有跟显示有关的修复方式，但是至今仍然没有办法完全解决这个问题

唯一能短暂解决问题的方法就是重启一遍Jellyfin服务

我也一直想不通，核显就在那里，Windows也是作为宿主机使用的，为什么就是检测不到核显呢

要是只是这样我可能还觉得是Jellyfin的问题

然而，同样是Win11，同样是Jellyfin，别人的intel小主机就是可以做到完美解码，只有我的AMD小主机出现了问题

作为一个服务器，却没有办法提供稳定的服务，这是非常令人失望的

因此，在购买了Mac mini后，我计划将Jellyfin服务转移到Mac mini上

## 田牌系统的无奈

当初为了方便上手，我给电脑安装了Windows 11作为宿主系统，但是作为服务器，我们必须承认Windows还是差了点

首先最大的问题还是目前Linux类系统的服务器仍然占大头，所以网络上的大部分服务器相关教程都是基于Ubuntu或其他Linux类系统或者MacOS的，Windows系统相关的教程仍然较少，因此在部署一些服务的时候需要自行探索

其次，在服务器上经常使用的Docker在Windows端的Docker Desktop也并不好用，一方面配置起来相对麻烦，另一方面也存在着各种难以找到教程解决的小问题

因此，我打算先保持现状凑合一下，等到购买了Mac mini后，就将这台小主机刷成Linux系统，再在上面部署一台Windows虚拟机

这样子就可以同时兼顾Linux和Window的优势了

不过距离购买Mac mini还有很长一段时间，再急也只能忍忍了