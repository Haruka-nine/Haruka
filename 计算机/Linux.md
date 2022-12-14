# 文件系统


![img](https://img-blog.csdnimg.cn/5caa8d8818854c06ad8c4388d73510e3.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAZ2hfeGlhb2hl,size_20,color_FFFFFF,t_70,g_se,x_16)

# VIM编辑器

## 基本操作

进入后默认为普通模式，按i进入编辑模式

## 普通模式

以 vim 打开一个档案就直接进入一般模式了（这是默认的模式）。在这个模式中， 你可 以使用『上下左右』按键来移动光标，你可以使用『删除字符』或『删除整行』来处理档 案内容， 也可以使用『复制、粘贴』来处理你的文件数据。

常用语法

| 语法                      | 功能描述                                   |
| ------------------------- | ------------------------------------------ |
| yy                        | 复制光标当前一行                           |
| y+shift+4 （$）           | 复制当前到行尾                             |
| y+shift+6（^）            | 复制当前到行头                             |
| y 数字 y                  | 复制一段（从第几行到第几行）               |
| p                         | 箭头移动到目的行粘贴                       |
| u                         | 撤销上一步                                 |
| dd                        | 删除光标当前行                             |
| d 数字 d                  | 删除光标（含）后多少行                     |
| x                         | 剪切一个字母，相当于 del，剪切当前光标内   |
| X                         | 剪切一个字母，相当于 Backspace，剪切光标前 |
| r                         | 对光标当前进行替换进行替换                 |
| R                         | 进入替换模式--相当于一种特殊的编辑模式     |
| w                         | 移动到下一个词（词头）                     |
| e                         | 移动到当前词词尾（或者下一个词词尾）       |
| yw                        | 复制一个词                                 |
| dw                        | 删除一个词                                 |
| shift+6（^）              | 移动到行头                                 |
| shift+4 （$）             | 移动到行尾                                 |
| 1+shift+g（shift+g就是G） | 移动到页头，数字                           |
| shift+g                   | 移动到页尾                                 |
| 数字+shift+g              | 移动到目标行                               |
| :set nu                   | 显示行号                                   |
| :set nonu                 | 不显示行号                                 |

## 编辑模式

一般模式是没办法编辑文件内容的，只能进行删除、复制、黏贴等的动作，但却是无法编辑文件内容的，需要按下 i I o O a A 等任何一个字母后才会进入编辑模式

i是当前光标前

a是当前光标后

o是当前光标行的下一行

I光标所在行的最前

A光标所在行的最后

O光标所在行的上一行

## 命令模式

使用:或者 /

| :w            | 保存                             |
| ------------- | -------------------------------- |
| :q            | 退出                             |
| :wq           | 保存并退出                       |
| :q!           | 不保存强制退出                   |
| :!            | 强制执行                         |
| /要查找的词   | n 查找下一个，N 往上查           |
| :noh          | 取消高亮显示                     |
| :set n        | 显示行号                         |
| :set nonu     | 关闭行号                         |
| :s/old/new    | 替换当前行匹配到的第一个         |
| :s/old/new/g  | 替换到当前行匹配的到的所有       |
| :%s/old/new   | 替换每一行的第一个old为new       |
| :%s/old/new/g | 替换内容 /g 替换匹配到的所有内容 |
# 虚拟机Linux系统安装
详细安装可以看尚硅谷，一般使用简易安装--选用一个镜像一直下一步就行

## 静态ip创建

VMware提供了三种网络连接模式
**桥接模式**
虚拟机直接连接外部物理网络的模式，主机起到了网桥的作用。这种模式下，虚拟机可以直接访问外部网络，并且对外部网络是可见的
在这种情况下，虚拟机和主机是平等的，这样简单，但一个路由器分配的子网是有限的，会导致ip分配不够


**NAT模式**
虚拟机和主机共建一个专用网络，并通过虚拟地址转换NAT设备对IP进行转换。虚拟机通过共享主机IP可以访问外部网络，但外部网络无法访问虚拟机

NAT模式就是建立一个局域网，创建一个虚拟路由，连接到主机上。通过这个虚拟路由，虚拟机就可以访问外网，那么主机如何访问虚拟机呢？
主机虚拟出一张网卡（vmnet8），连接到虚拟路由上，这时主机就和虚拟机在一个局域网了

假设 设置子网ip为 192.168.10.0   子网掩码为255.255.255
这时的主机的网卡ip地址设置为第一个  192.168.10.1
路由也就是网关的ip地址设置为 192.168.10.2
dns也设置为 192.168.10.2

那么虚拟机就可以在192.168.10.3~192.168.10.254之间选择ip地址进行静态绑定

**仅主机模式**
虚拟机只与主机共享一个专用网络，与外部网络无法通信

我们使用NAT模式

![[Pasted image 20220928091931.png]]
我们需要更改vm的网络设置中的NAT模式的子网ip和子网掩码，比如设置成192.168.10.0，子网掩码为255.255.255.0

其中Nat设置中网关ip更改
![[Pasted image 20220928092324.png]]


将主机的网络中的Vm8虚拟网卡设置更改为下面，注意要对应
![[Pasted image 20220928092158.png]]



首先我们进入root账号登陆，在终端中输入以下命令

``vim /etc/sysconfig/network-scripts/ifcfg-ens33 ``

将BOOTPROTO的dhcp(自动获取)改为static
并在下方添加
IPADDR=192.168.10.10
GATEWAY=192.168.10.2
DNS1=192.168.10.2

这个IPADDR就是ip地址,最后一位可以根据需要自行更改，然后就可以用这个地址在主机远程访问了

**注意**
图形化界面服务，默认会安装 NetworkManager 管理服务，NetworkManager服务启动以后导致系统内部的网络配置出现紊乱。如果使用后发现网络不通要关闭NetworkManager，并禁止其开机自启

# 系统命令

## systemctl
```
1、启动服务 systemctl start 服务名

2、停止服务 systemctl stop 服务名

3、重启服务 systemctl restart 服务名

4、查看服务是否已启动 systemctl is-active 服务名

5、查看服务的状态 systemctl status 服务名

6、启用开机自启动服务
systemctl enable 服务名

7、停用开机自启动服务
systemctl disable 服务名
```




