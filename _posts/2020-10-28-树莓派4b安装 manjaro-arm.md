---
title: 树莓派4b安装 manjaro-arm
tags: Linux 树莓派
---

![简单配置后的 Manjaro](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-5f05a0020a484520.png)

# 为什么使用manjaro？

首先，manjaro系统基于Arch Linux，在manjaro上有体现以下优点：

- **优点： 无需繁琐系统升级**
   Arch Linux 采用滚动升级模型，简直妙极了。这意味着你不需要操心升级了。一旦你用上了 Arch，持续的更新体验会让你和一会儿一个版本的升级说再见。只要你记得‘滚’更新（Arch 用语），你就一直会使用最新的软件包们。

  **缺点： 一些升级可能会滚坏你的系统**
   但manjaro官方会在滚动更新时进行稳定性测试，极大增强了稳定性

- **优点： Arch Wiki 无敌**

  [Arch Wiki](https://wiki.archlinux.org/) 是一个无敌文档库，几乎涵盖了所有关于安装和维护 Arch 以及关于操作系统本身的知识。Arch Wiki 最厉害的一点可能是，不管你在用什么发行版，你多多少少可能都在 Arch Wiki 的页面里找到有用信息。这是因为 Arch 用户也会用别的发行版用户会用的东西，所以一些技巧和知识得以泛化。
   因为manjaro基于Arch Linux，所以文档通用

- **优点：Arch 用户软件库 （AUR）**

  [Arch 用户软件库](https://wiki.archlinux.org/index.php/Arch_User_Repository)Arch User Repository （AUR）是一个来自社区的超大软件仓库。如果你找了一个还没有 Arch 的官方仓库里出现的软件，那你肯定能在 AUR 里找到社区为你准备好的包。
   AUR 是由用户自发编译和维护的。Arch 用户也可以给每个包投票，这样后来者就能找到最有用的那些软件包了。
   因为AUR是由用户自发编译和维护的，所以相较于官方仓库来说，稳定性不够好

- **优点：软件最齐全**
   有赖于Arch Linux的基础，manjaro和Arch Linux实现了**最全的软件库**，没有之一
   **缺点：有的软件你可能找不到**
   因为manjaro中的小部分软件包和其他系统的软件包名字可能有所差异，所以有可能你找不到（你找不到不代表没有，也不代表别人找不到，也有可能属于某个完整的软件包）

- **优点：你永远可以用到最新的软件**
   举个小例子，截至2020年10月8日，Ubuntu20.04（更新到最新）的默认python版本为3.8.2，系统仓库的`python3-django`的版本为1.9
   与此同时manjaro的python版本为3.8.5，系统仓库的`python-django`的版本则为3.1.1
   总结：你没有的我有，你有的我比你新

  **缺点：太新了🤣**
   一些**第三方**插件或软件可能跟不上manjaro的步伐

- **优点：免于编译的苦恼**
   如`opencv`，在大部分其他系统上，你需要手动编译，并有可能产生大量的依赖项错误，但在manjaro中，你只需要执行`pacman -S opencv`，去喝口水，`opencv`相关的库就已经安装好了。
   再比如python的`pandas`和`numpy`，如果使用pip进行安装，你会花费不短的一段时间等待编译（数十分钟），如果用`apt`进行安装，你甚至有可能安装到差一个大版本的软件包，但在manjaro中，你只需要执行`pacman -S python-numpy python-pandas`，相关的库和依赖就轻松的安装成功

# 准备

1. 树莓派4或4b（**其他版本的树莓派无法安装**）
2. 烧录软件 [Etcher](https://www.balena.io/etcher/)
3. 大小最少为16G的tf卡，速度越快越好。
    虽然最低要求是 C10（连续写入速度**最低**为 10Mb/s），但个人还是推荐 V30（连续写入速度**最低**为30Mb/s）
    如果你比较富裕，可以选择固态硬盘转 USB3.0，使用 USB 启动系统
4. 连接树莓派的屏幕（安装好后的初始化设置需要屏幕）
5. 键鼠（最少有键盘）

# 下载镜像

首先进入manjaro-arm的官方发布页。[链接](https://forum.manjaro.org/t/manjaro-arm-21-04-released/61974)
 如果速度感人，你可以使用[**代理工具**](https://sockboom.mobi/auth/register?affid=108759)进行加速

可以看到树莓派4的镜像有4个：

![不同的桌面](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-73d6bdc0ebdc22a9.jpg)

这四个系统镜像有什么区别呢？

- XFCE：占用内存较小（下面会给出各系统的内存占用共参考），但是图形界面不好看，系统自带的功能不丰富
- KDE Plasma：界面好看，有很多好用小功能（自带），如桌面小部件、可是化的开机启动项管理，缺点是内存占用相较**XFCE**占用**略大**（具体看表格）
- i3：一个纯命令行操作的图形界面系统，不熟悉Linux命令的就别装了
- Sway：i3的美化加强版，不熟悉Linux命令的别装

**系统占用表格：**

| 测试场景               | XFCE  | KDE Plasma | i3    | Sway  |
| ---------------------- | ----- | ---------- | ----- | ----- |
| 图形界面开机（登录前） | 378Mb | 447Mb      | 321Mb | 386Mb |
| 图形界面开机（登录后） | 717Mb | 728Mb      | 423Mb | 517Mb |
| 命令行界面开机         | 235Mb | 223Mb      | 230Mb | 240Mb |

> 可以看出 KDE Plasma 的内存占用只比 XFCE 高了十多Mb，我个人还是推荐安装 KDE Plasma
> 
>  接下来的操作我会用 KDE Plasma，操作基本都一样，但在安装中文输入法后，设置开机启动项时，XFCE需要命令行添加

# 烧录系统

使用 Etcher 进行烧录

![Etcher](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-509c496e220de132.jpg)

这三步分别是选择镜像、选择要烧录的设备、进行烧录。傻瓜式操作，不做赘述

# 初始化设置

> 在第一次开机时不要设置超频，会无法开机
> 
> 没有屏幕的可以想办法找到树莓派的IP，第一次开机后使用 root 用户 ssh 进系统也可以进行设置：`ssh root@192.168.x.x`

在第一次开机后会要求进行初始化设置：

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-e3716be647a44fc2.jpg)

首先选择键盘布局，选择**us**

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-48aa8ee367bd001a.jpg)

输入用户名（**只能小写**）

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-12312e9d6a7dae53.jpg)

设置用户组（**直接回车确认**）

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-cbd5c9505b204945.jpg)

 字面意思，输入**全名**（可以理解为昵称）

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-a9d775e136929907.jpg)

设置密码

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-f2e091328db39794.jpg)

确认密码

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-5230abb8c4bcd157.jpg)

输入root密码

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-8a3a705e657f9af4.jpg)

确认root密码

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-defc4c0949d27747.jpg)

选择时区（Asia/Shanghai，亚洲/上海）

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-d5e6f22440981192.jpg)

选择语言（zh_CN.UTF-8，中文.UTF-8）

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-f15b585bbf4e3751.jpg)

输入hostname，不懂什么意思的输入pi

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-88c65efc3b233d83.jpg)

然后进行信息确认，没有问题选 yes，有问题选 no 重新填

接下来系统会自动重启进行初始化配置，有的板子可能需要10~30分钟才能开机，**请耐心等待**

开机后我们会发现中文字体都变成了方块，这时不要着急，我们只需要安装中文字体并重启就好了
 这个问题我已经和官方反馈了，官方将在下个版本(20.10)自带中文字体

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-f38e993674e99025.jpg)

输入密码回车进入系统

# 安装中文字体

以下就是盲人摸象般的操作
 首先我们要连接Wi-Fi，点击右下角网络连接的图标

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-b6e8d63a5a0588a1.png)

会扫描Wi-Fi，点这个连接Wi-Fi，输入密码

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-6710f727316d458f.png)

左下角打开命令行

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-16e788a0a69b9d5a.png)

运行以下代码（根据提示输入**root密码**）：

> Manjaro是基于Arch Linux的系统，使用`pacman`而不是`apt`命令来安装软件
>  在后面我会给出`pacman`的常用操作供参考学习

```shell
sudo pacman-mirrors --country China    # 换源
sudo pacman -Syu    # 系统更新，manjaro是滚动更新方式，第一次更新很大，但一定要更新
sudo pacman -S wqy-microhei    # 必须安装，否则无法正常显示中文
sudo pacman -S wqy-bitmapfont wqy-microhei-lite wqy-zenhei    # 同字族，可选安装
sudo pacman -S neovim    # neovim，vim的新版，安装中文输入法时要用，你也可以使用vim
reboot    # 重启树莓派
```

重启后就可以看到可以正常显示中文了

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-9e3d23a2dbcfcadd.jpg)


# 安装中文输入法

打开命令行，输入以下命令：

```shell
sudo pacman -S fcitx5 fcitx5-chinese-addons fcitx5-qt fcitx5-gtk kcm-fcitx5
# 安装中文输入法及依赖项
```

接下来要设置输入法的配置文件。如果**你会使用并已经安装**`nvim`或`vim`，请在命令行输入以下命令：
 不会用就百度简单学一下

```shell
# 不要用root用户执行
cd ~/
nvim .xprofile
```

如果你不会使用`vim`，执行以下命令：

```shell
# 不要用root用户执行
cd ~/
kate .xprofile    # 没有Kate就安装
```

然后将配置文件输入并保存：

```bash
#fcitx
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
```

配置输入法开机自启
 摸索一下找到系统设置，点击开机于关机

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-6fa482970dcab65c.png)

点击

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-79ecf92fab02f4c4.png)

点击添加程序，输入fcitx找到输入法，选择工具中的`Fcitx 5`，点击确定

![img](/assets/images/2020-10-28-树莓派4b安装 manjaro-arm/13919472-e053dd6d6cb763f1.png)

重启树莓派，使用快捷键`Ctrl + 空格`就可以唤醒输入法

# pacman常用命令

```shell
pacman-mirrors -c China            # 更新为中国源
pacman -Sy        # 更新软件库(类似apt update)
pacman -Syu        # 更新系统
pacman -Ss 软件包        # 在网络上查找软件包
pacman -Qs 软件包        # 查询本地已安装的软件包
pacman -S 软件包        # 安装软件包
pacman -Sc              # 清除安装包缓存
pacman -R 软件包        # 卸载软件
pacman -Rs 软件包        # 卸载软件以及依赖项
pacman -R $(pacman -Qdtq)    # 卸载孤包
```

## 小彩蛋

 在`/etc/pacman.conf`文件的最后添加`ILoveCandy`，`pacman`的进度条会变成吃豆人。解除文件中`Color`的注释后提示信息会有颜色，进度条的吃豆人也会变成黄色。

# 最后

1. 因为Arch Linux良好的仓库，建议安装python库时使用`pacman`进行安装，此后你便不用关心python库的升级，只需要每隔一段时间正常更新系统即可
    假如现在需要安装python的`pillow`库，只需要在库名前加上`python-`，这便是它的包名，运行`pacman -S python-pillow`即可安装
2. 在Manjaro中，其他系统中的各个小软件包将合并为集成包，如Ubuntu下安装`sqlite3`，你可能需要安装`libsqlite3`、`sqlite3-dev`等一些包，但在Manjaro中你只需要`pacman -S sqlite3`，就将安装`sqlite3`的运行库和开发库

