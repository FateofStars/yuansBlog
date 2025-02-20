# 小米路由器3刷老毛子并使用USB进行网络共享

## 参考文档
[CSDN-小米路由器3刷机](https://blog.csdn.net/s_daqing/article/details/116398731)

[恩山无线论坛-小米路由器3（Xiaomi Mi Router R3）刷openwrt](https://www.right.com.cn/forum/thread-8287327-1-1.html)

[浏览器记录logo-手机共享网络给Padavan老毛子USB调制解调器设置,Padavan老毛子USB调制解调器](https://ioozu.com/post-3137.html#gsc.tab=0)

## 文件下载

下列链接均为固件下载链接，点击即可下载

[小米路由器开发板2.11.20固件](http://bigota.miwifi.com/xiaoqiang/rom/r3/miwifi_r3_all_55ac7_2.11.20.bin)

[pb-boot固件](http://downloads.pangubox.com:6380/pb-boot/18.07.25/pb-boot-xiaomi3-20180724-ef73b47.img)

[老毛子padavan固件](https://opt.cn2qq.com/padavan/MI-R3_3.4.3.9-099.trx)

## 使用工具
[putty](https://www.putty.org/?hl=zh-cn)

[WinSCP](https://winscp.net/eng/docs/lang:chs)

牙签一根

---

## 实机操作

- **在刷写固件之前，建议断开其他所有网络连接，以免出现意外状况**

#### 固件降级并开启SSH

- 进入路由器后台，选择下载好的固件进行升级

- 升级完成后，在网址栏找到你的stock，并进行复制和记录
```
(参考stok，每台设备不一样)stok=5c392400a7a61ef3e3b72e060dc769d8
```

- 接下来，在浏览器中新建一个标签页，在地址栏输入
```
http://192.168.31.1/cgi-bin/luci/;stok=(输入你的stok)/api/xqnetwork/set_wifi_ap?ssid=tianbao&encryption=NONE&enctype=NONE&channel=1%3Bsed%20%2Di%20%22%3Ax%3AN%3As%2Fif%20%5C%5B%2E%2A%5C%3B%20then%5Cn%2E%2Areturn%200%5Cn%2E%2Afi%2F%23tb%2F%3Bb%20x%22%20%2Fetc%2Finit.d%2Fdropbear
```

网页出现
>“{“msg”:“未能连接到指定WiFi(Probe timeout)”“code”:1616}”

则表示命令执行成功

- 然后，在地址栏输入
```
http://192.168.31.1/cgi-bin/luci/;stok=(输入你的stok)/api/xqnetwork/set_wifi_ap?ssid=tianbao&encryption=NONE&enctype=NONE&channel=1%3B%2Fetc%2Finit.d%2Fdropbear%20start
```

网页出现
>“{“msg”:“未能连接到指定WiFi(Probe timeout)”“code”:1616}”

则表示命令执行成功

- 最后，在地址栏输入
```
http://192.168.31.1/cgi-bin/luci/;stok=(输入你的stok)/api/xqsystem/set_name_password?oldPwd=当前后台管理密码&newPwd=新的后台管理密码
```

网页出现
>“{“code”:0}”

则表示命令执行成功

- **至此，就完成了对路由器开启SSH的步骤**

---

#### 刷写PandoraBox

- 开启putty，在Host Name中，输入路由器的地址（可在网络设置、属性、IPv4 DNS服务器中进行查看），然后点击**Open**建立连接
<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/MIR3/putty.png">

- 在 **login as**选项中，输入root并回车
- 在 **Password**选项中，输入你在**newPwd**中输入的密码

- 接下来，在命令行中输入
```ssh
sudo nvram set uart_en=1
sudo nvram set flag_last_success=1
sudo nvram set boot_wait=on
sudo nvram commit
```
*注：在需要输入密码时，请再次输入你的密码*

- 不要关闭putty，打开WinSCP
<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/MIR3/WinSCP.png" width=500>
- 文件协议选择**SCP**，小米路由器3由于过老不支持SFTP
- 在主机名中输入路由器的地址
- 用户名为**root**，密码为你在**newPwd**中输入的密码
- 连接成功后，将pb-boot文件上传到**tmp**文件夹

- 上传完成后，回到putty，输入以下命令
```ssh
cd /tmp
sudo mtd_write write (pb-boot的文件名) Bootloader
sudo reboot
```

- 等待路由器重启完成后，PandoraBox大概率就已经刷写完成，此过程需要约两分钟
- 此时，我们断开路由器电源，使用牙签抵住Reset按钮，同时接上路由器电源
- 等待几秒钟后，路由器状态灯开始闪烁橙色
- 此时，我们在网络与共享中心中可以看到路由器的IP地址已经变为192.168.1.1
- 我们在路由器中输入此地址，就可以看见Pandora的界面
<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/MIR3/Pandora.png" width=500>
- **此时，潘多拉已刷写完成**

---

#### 刷写老毛子固件
- 打开潘多拉界面，点击**选择文件**
选择刚刚下载好的padavan固件，点击**恢复固件**

- 等待进度条走完与路由器重启
- *注：我们可以打开网络与共享中心查看IPv4 DNS服务器地址
等到电脑获取到DNS服务器地址后，即表示路由器已刷机完成*

---

#### 登录后台管理界面并配置USB网络共享

- 再次将新获取的地址(一般为192.168.123.1)输入到浏览器中，会弹出路由器的认证窗口
<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/MIR3/padavanloginin.png" width=300>

- 在用户名和密码都输入**root**，点击**登录**即可进入后台管理界面

- 接下来点击**高级设置**，点击进入**USB调制解调器**
- 开启**启用USB调制解调器**按钮
- 将**调制解调器**类型选择为**NDIS：LTE and other**
- **国家**选择为**China**
- **ISP**选择为你所使用的运营商
- **首选网络**选择为**LTE->UMTS**
- 开启**自动获取DNS**
- 未提及的选项保持默认
- 确保无误后，点击**应用本页面设置**
<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/MIR3/USBMorden.png" width=500>

- 在完成以上设置后，将你的手机或支持USB网络共享的随身WIFI插到路由器的USB接口上，并开启USB网络共享
等待一段时间后，就可以正常上网了
<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/MIR3/padavanwebmap.png" width=300>

---

- **至此，小米路由器3刷老毛子并使用USB进行网络共享的教程全部完成**