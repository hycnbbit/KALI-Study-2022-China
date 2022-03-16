# 从零开始学KALI

## 1.安装KALILinux系统

### 下载Kali镜像

安装kali的方法有很多，但最常用的是在虚拟机，物理机，和docker中安装KALI系统

而我们推荐在虚拟机安装kali系统，利用虚拟机可以借助虚拟机的快照功能，用来备份恢复kali系统。

**KALI的官方网站： [Kali Linux | Penetration Testing and Ethical Hacking Linux Distribution](https://www.kali.org/) **

我们选择downloads选项，向下滑动即可看到KALI系统的iso镜像，其中包含4个版本，

![1647411339305](C:\Users\hac\AppData\Roaming\Typora\typora-user-images\1647411339305.png)

分别为Installer（通过自定义完成脱机安装）Weekly（每周更新具有最新的未经测试的镜像---可能不稳定）Everything（包含所有可能的工具）NetInstaller（网络镜像---所有的软件包都在安装过程中下载）

由于KALI的镜像存放在国外的服务器中下载较慢可以使用国内的克隆网站例如中科大开源镜像站（ [USTC Open Source Software Mirror](https://mirrors.ustc.edu.cn/) ），清华大学开源镜像站。中科大开源镜像站速度快于清华大学开源镜像站，而且界面明显，在主界面点击获取安装镜像，选择kali linux 和发行版本，建议下载Installer版本。![1647411901091](C:\Users\hac\AppData\Roaming\Typora\typora-user-images\1647411901091.png)

### 下载虚拟机软件

虚拟机是借助于CPU虚拟化功能实现的如要使用虚拟机需要进入BIOS打开CPU虚拟化，部分笔记本默认打开。常见的虚拟机软件有 WindowsHyper-V  [Oracle VM VirtualBox](https://www.virtualbox.org/) （开源免费，界面比较复杂，不适合新手）VMware Workstation Player (免费不开源)且有商业版本VMware Workstation Pro 而两者差别不大此教程所使用的软件为VMware Workstation Pro ，建议大家支持正版，下载正版有三十天的试用期(下载Pro版本会自带VMware Workstation Player无需重新下载)

### 开始安装

打开虚拟机软件（这里以VM16pro为例其他虚拟机软件创建过程相似）选择创建新的虚拟机，选择典型，再选择你下载的镜像![1647412747136](C:\Users\hac\AppData\Roaming\Typora\typora-user-images\1647412747136.png)

选择Linux 版本为Debian10.x64位

建议为虚拟机分配40GB的磁盘空间（分配40GB并不会立马占用四十GB，而是按照镜像的实际大小）完成后开启虚拟机即可

选择第一项图像化安装![1647412966987](C:\Users\hac\AppData\Roaming\Typora\typora-user-images\1647412966987.png)

进入虚拟机后如要进入物理机可以按快捷键Ctrl+Alt即可返回物理机

之后按照步骤进行安装即可在磁盘分区时直接使用默认设置即可，在最后选择是即可完成分区。

![1647413901069](C:\Users\hac\AppData\Roaming\Typora\typora-user-images\1647413901069.png)

建议桌面环境选择GNOME桌面，默认的Xfce桌面无法使用全局代理

## 2.配置KALI

### 安装VMwareTools

VMwareTools可以实现虚拟机与物理机之间的文件传输和复制粘贴

一般vmware虚拟机安装完成后就会自动给虚拟机安VMwareTools

### 为KALI跟换软件源

打开终端

使用Vim编辑 /etc/apt/sources.list 文件, 在文件最前面添加以下条目：

```bash
┌──(kali㉿kali)-[~]
└─$ sudo vim /etc/apt/sources.list

我们信任您已经从系统管理员那里了解了日常注意事项。
总结起来无外乎这三点：

    #1) 尊重别人的隐私。
    #2) 输入前要先考虑(后果和风险)。
    #3) 权力越大，责任越大。

[sudo] kali 的密码：

```

"linux输入密码时不会显示任何东西直接输入就可以密码是你安装时创建的账户的密码"

![1647417383990](C:\Users\hac\AppData\Roaming\Typora\typora-user-images\1647417383990.png)

按字母i进入编辑模式

在KALI的默认源前添加”#“代表注释

再把下面的链接复制到文件中去，Linux中的粘贴快捷键为Ctrl+Shift+V

```bash
deb https://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
deb-src https://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
```

![1647417560916](C:\Users\hac\AppData\Roaming\Typora\typora-user-images\1647417560916.png)

按键盘上的ESC键退出编辑模式再安”：“冒号键输入wq回车保存退出，注意看这张图的最下面就是输入的：wq

![1647417658923](C:\Users\hac\AppData\Roaming\Typora\typora-user-images\1647417658923.png)



更改完 sources.list 文件后请运行 sudo apt update 更新索引以生效。

### 更新KALI

通常安装的KALI不是最新的版本我们需要执行

```bash
┌──(kali㉿kali)-[~]
└─$ sudo apt upgrade 

```

进行更新，更新可能升级内核，更新完后建议重启虚拟机，以使更新的内核生效。

如果更新过程中遇到这个问题

```bash
下列软件包有未满足的依赖关系：
 libwacom9 : 依赖: libwacom-common (= 2.1.0-2) 但是 1.12-1 正要被安装
E: 破损的软件包

```

可以执行如下命令

```bash
sudo apt install libwacom9 libwacom2-
```

之后再重新执行更新命令

### 备份虚拟机

![1647418314858](C:\Users\hac\AppData\Roaming\Typora\typora-user-images\1647418314858.png)

点击虚拟机 > 快照 > 拍摄快照

![1647418431484](C:\Users\hac\AppData\Roaming\Typora\typora-user-images\1647418431484.png)

可以在此对拍摄的快照填写相关描述

如要恢复快照，点击虚拟机 > 快照 > 快照管理器

![1647418547143](C:\Users\hac\AppData\Roaming\Typora\typora-user-images\1647418547143.png)

再选择拍摄的快照点击转到即可。