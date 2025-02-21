# 在 Windows 11 上部署 Material for MKDocs ，并将网站托管在 github 上的同时，使用 Custom domain 将网页挂载到自己的域名下

## 参考文档
[MkDocs-Getting Start](https://www.mkdocs.org/getting-started/)

[Bilibili-NextDeep-MkDocs建站教程](https://space.bilibili.com/388561300/lists/4601498?type=series)

[Material for MkDocs-Steup](https://squidfunk.github.io/mkdocs-material/setup/)

## 软件/依赖下载
[python](https://www.python.org/downloads/)

[git](https://git-scm.com/downloads)

[VSCode](https://code.visualstudio.com/)

---

## 环境部署

### 创建存储路径
- 在任意目录下**新建文件夹**，命名为你想要的名称(推荐直接命名为MkDocs)

### 安装VSCode，并检查环境
- 双击下载下来的VSCode安装包，一直点**下一步**直到安装完毕(*注：可根据个人需求修改安装选项*)

- 进入VSCode，点击顶部导航栏的**Terminal**，选择**New Terminal**，此时下方TERMINAL显示内容为
> PS C:\Users\ (用户名)>

#### 检查python环境
- 在TERMINAL中输入
```
python --version
```

- 若返回
> Python (版本号)

则说明Python已在电脑中准备就绪，可跳过**安装并配置pythom**

- **本次教程基于Python 3.13.2进行搭建，若版本低于3.13.2可升级至最新版本或3.13.2以减少意外情况发生的可能**

- 若没有返回，则说明电脑中未安装好python

#### 检查git环境

- 在TERMINAL中输入
```
git --version
```

- 若返回 
>git version (版本号)

则说明git已经安装好，可跳过**安装并配置git**

- 若返回
>git: The term 'git' is not recognized as a name of a cmdlet, function, script file, or executable program.
>Check the spelling of the name, or if a path was included, verify that the path is correct and try again.

则说明电脑中未安装好git

### 安装python

- 双击打开python安装文件

- 勾选**Add Python.exe to PATH**

<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/MkDocs/pythoninstall.png">

- 点击**INSTALL NOW**

- 等待进度条走完，回到VSCode

- 在TERMINAL中输入
```
python --version
```

- 若返回
> Python (版本号)

则说明Python已在电脑中准备就绪

- **若安装时提示“请联系管理员”，请退出程序，使用管理员权限运行后重新安装**

### 安装git
- 双击打开git安装文件

- 一直点击**Next**(*注：可根据个人需求修改安装选项*)，直到**Choosing the dafault editor used bug Git**

- 在此项目中选择**Use Visual Studio Code as Git's default editor**选项

<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/MkDocs/gitinstall.png" width=500>

- 点击**Next**，来到**Adjuesting the name of the initail branch in new repositories**

- 勾选**Override the default branch name for new repositories**，下方输入框保持**main**不变

<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/MkDocs/gitinstall1.png" width=500>

- 一直点击**Next**(*注：可根据个人需求修改安装选项*)，等待读条完毕，回到VSCode中

- 在TERMINAL中输入
```
git --version
```

- 若返回 
>git version (版本号)

则说明git已经就绪

---

## 安装并初步配置MkDocs

*注：接下来的操作默认都会在VSCode中进行，默认将代码输入到TERMINAL中*

*下列操作可能需要一定的网络基础*

### 升级pip

- 输入
```
python -m pip install --upgrade pip
```

等到输入框再次出现，则表明已升级完成

### 安装MkDocs 与 Material for MkDocs

- 输入
```powershell
pip install mkdocs
```

等到输入框再次出现，若没有报错则表明MkDocs已下载完成

- 输入
```
pip install mkdocs-material
```

等到
```
PS C:\Users\ (用户名)
```
再次出现，若没有报错则表明Material for MkDocs已下载完成

### 创建MkDocs项目

- 找到在步骤**创建存储路径**创建的路径，复制后，输入

```
cd (创建的路径)
```

可以看到
```
PS C:\Users\ (用户名)
```
已经变更为了

```
PS (创建的路径)
```

**我们后续的操作都将在此路径下完成**

- 接下来创建MkDocs项目，输入
```
mkdocs new (项目名称)
```

来创建新项目

- 创建成功后，我们在路径中就可以看见一个以项目名称命名的新文件夹，打开文件夹就可以看到下面内容

>docs 文件夹

>mkdock.yml

- 接下来，我们再次
```
cd (项目名称)
```
进入创建的项目

- 此时，我们输入
```
mkdocs serve
```
即可在127.0.0.1:8000中预览项目(*注：具体地址参考**Serving on**后面的IP地址*)

- 在我们保存更改后，即可在此地址中查看最新变化

- 按下**Ctrl+C**即可停止服务

### 更改MkDocs主题为Material并进行自定义设置

- 在导航栏中，选择**File**，点击**Open Folder**，选择打开你的项目文件夹

- 此时，会弹出一个窗口询问是否信任文件夹

<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/MkDocs/VSCtrustfolder.png" width=500>

- 点击**Yes, I trust the authors**，否则我们无法编辑其中的文档

- 我们打开其中的**mkdocs.yml**，可看见内容如下
>site_name:My Docs

- 想要启用Material for MkDocs，我们就需要在文档中输入主题配置

```yml
theme: 
    name: material
```

- 保存好文件，我们打开预览网页就可以看到，页面已经变化为了Material for Mkdocs的经典样式

- 我们可以通过更改**site_name**后的文字来修改站点名称

- 通过添加

```yml
theme: 
    language: zh
```

将网站的默认语言改为中文

- 还可以通过
```yml
copyright: (版权信息)
```
为网页添加版权信息

>**更多自定义设置请参考 [Material for MkDocs-Setup](https://squidfunk.github.io/mkdocs-material/setup/)**

---

## 将网站托管到GitHub中，并配置Custom domain

*注：接下来的操作默认都会在VSCode中进行，默认将代码输入到TERMINAL中*

*下列操作可能需要一定的网络基础*

### 创建GitHub项目仓库

- 注册好GitHub账号后，进入个人主页，在左侧的**Top repositories**中点击**New**进行项目创建

- 创建时，我们可以创建两种类型的GitHub仓库，一种为user类型，一种为project类型

>- user类型的仓库可以直接通过 用户名.github.io 来访问，一个账户只能创建一个
>- project类型的仓库需要通过 用户名.github.io/项目名称 来访问，一个账户能创建多个

- 两种类型的仓库创建方式的区别仅在于**Repository name**

>- 要创建user类型的仓库，则在**Repository name**中输入 **用户名.github.io**
>- 要创建project类型的仓库，则在**Repository name**中输入**项目名**即可

- 其他项目保持不变，点击**Create repository**即可完成创建

### 使用git将网站内容添加到GitHub仓库

- 回到VSCode的TERMINAL，我们接下来输入以下命令，使git记录我们的更改

```
git init
git add .
git commit -m"(注释)"
```

- 接下来，我们就要准备将文件推送到GitHub上了
- 我们返回到GitHub上新创建的项目，可以在**or push an existing repository from the command line**选项下面看见

```
git remote add origin https://github.com/(用户名)/(仓库名).git
git branch -M main
git push -u origin main
```

- 将这三行命令依次复制到TERMINAL中执行

>- 第一次执行命令时，可能会弹出窗口**Connect to GitHub**
<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/MkDocs/connecttogithub.png" width=300>
>- 我们点击**Sign in with your browser**
>- 然后在弹出的浏览器中登录GitHub账户，并点击**Authorize git-ecosystem**
<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/MkDocs/authorizegit.png" width=300>
>(*注：在完成操作后这个授权页面可能会无法正常加载，无需担心，你已完成了登录，可以关掉网页回到VSCode了*)

- 如果返回内容最后显示
```
To github.com:(用户名)/(仓库名).git
*[New branch]     main -> main
branch 'main' set up to track 'origin/main'
```
 
则说明已经将更改推送到仓库

- 我们可以回到GitHub上查看文件是否已经出现

- 如果返回结果出现

>fatal: unable to access 'https://github.com/(用户名)/(仓库名).git': Failed to connect to github.com port 443 after ****** ms: Could not connect to server

或
>fatal: unable to access 'https://github.com/(用户名)/(仓库名).git': schannel: server closed abruptly (missing close_notify)

等错误，多半是网络问题，可多次重试直到成功

### 使用MkDocs进行网页渲染并配置Custom domain

#### 使用MkDocs对网页进行渲染

- 在成功将更改推送到仓库后，我们可以执行命令

```
mkdocs gh-deploy
```

来使MkDoccs对仓库中的文件进行渲染与挂载

>- 第一次运行命令时，肯能会看见
>```
>WARNING - Version check skipped: NO version specified in previous deployment
>```
>这是正常现象，无需担心

- 如果返回内容最后显示
>INFO - Your documentation should shortly be avaliable at: https://(用户名).github.io/(仓库名)/
 
则说明已经网页已成功构建

- 如果返回结果出现大段红紫色报错，我们可以往上找到**fatal**
- 若显示为

>fatal: unable to access 'https://github.com/(用户名)/(仓库名).git': Failed to connect to github.com port 443 after ****** ms: Could not connect to server

或
>fatal: unable to access 'https://github.com/(用户名)/(仓库名).git': schannel: server closed abruptly (missing close_notify)

等错误，多半是网络问题，可多次重试直到成功

- 网页构建完成后，我们就可以来到GitHub，在顶部导航栏点击**Actions**，就可以看见网页的构建进程
<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/MkDocs/pagebuilding.png" width=900>

- 等待到**pages build and deployment**左侧的黄色点变为绿色勾，我们的网站就完成部署了，此过程约需要半分钟

- 我们点击顶部导航栏的**Settings**，在左侧工具栏点击**Pages**，就可以看见你托管在GitHub上的网站的网址了

#### 使用Custom domain将网页挂载到自己的域名下

##### GitHub部分设置

- 在当前页面找到下面的**Custom domain**，在输入框中输入你的域名(*注：需自己在域名服务提供商上购买*)
- 填写的域名可以是二级域名，也可以是三级域名

>二级域名如：example.com

>三级域名如：test.example.com

- 点击**Save**，无需等待DNS Check完毕，看见**Your site is live at**后面的网址变为你设定的域名后，Custom domain在GitHub上就已经配置完成了

- 等待一到两分钟，到下方的**Enforce HTTPS**左侧的选择框由灰变白，则勾选该选项

<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/MkDocs/EnforceHTTPS.png">

##### 域名解析部分设置

- 接下来我们来到域名服务提供商的域名管理界面(*注：以阿里云为例*)，找到自己需要的域名，点击**解析**，进入云解析界面

- 点击**添加记录**
- **记录类型**选择**A**
- **主机记录**按照自己的需求填写，**需要跟在GitHub中填写的一致**
- **记录值**填写GitHub的IP地址(*注：为每一条地址添加一条解析，只需记录值有变化，其他不变*)

>185.199.108.153

>185.199.109.153

>185.199.110.153

>185.199.111.153

- 其他保持默认
- 点击**确定**，保存记录

<img src="https://caihsicloud.oss-cn-guangzhou.aliyuncs.com/BlogsPicture/tech/MkDocs/domain.png" width=300>

- 接下来我们访问添加的域名，就可以访问我们挂载的网页了

- **至此，我们就完成了这个教程的所有内容** 

---

## 后续问题

### 如何使用MkDocs


#### 编写文档

- MkDocs使用的是**MarkDown**语法，可以在各大平台中搜索相关教程视频
- 在学习MarkDown语法的过程中，我们可以利用自带的**index.md**来进行尝试

#### 新建文档与分类

- MkDocs的所有文档都将存在**docs**文件夹中，我们可以在文件夹中新建 .md 格式的文档来新建文档
- 同时，我们还可以在文件夹下新建文件夹来创建分类，一切编排随你喜好

### 在配置完成Custom domain后，再次推送报错

- 在配置完Custom domain后，可能会在下次push的时候会出现错误

>error: failed to push some refs to 'https://github.com/(用户名)/(仓库名).git'

>hint: Updates were reject because the remote contains work that you do not

>hint: the same ref. If you want to intergrate the remote changes, use

>hint: 'git pull' before pushing again.

>hint: See the 'Note about fast-forwards' in 'git push --help' for details

- 这是因为我们在配置Custom domain的时候，会在仓库中新增更改

- 我们需要先下载更改，再重新将我们的更新push上去
- 我们只需要依照提示，输入
```
git pull
```

将仓库中的更改下载到本地，再重新执行push即可完成更新