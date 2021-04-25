## 系统安装

1、下载sdk
首先，在 NVDIA 官网上下载并安装 SDK Manager（需要注册一个 nvida 账号才能下载）下载地址：https://developer.nvidia.com/embedded/downloads

2、安装sdk

输入命令：

```shell
sudo apt install sdk*.deb(换成你下载的安装包路径)
```

3、启动sdk

```shell
sdkmanager
```

### 踩坑记录

#### 1、SDKManager无法登陆以及OOPS问题

使用移动、联通宽带均无法直接登陆sdkmanager账户，使用魔法上网，情况稍有改善，但仍无法进入STEP2界面，出现OOPS网络连接不通问题。

解决方法：

- 通过手动配置sdkmanager客户端右上角settings内的代理（与魔法上网参数一致），偶然成功刷机一次，但该方法第二天失效。具体成功原因暂时未知。
- 使用学校的10GB流量账号可流畅登陆下载运行。、

#### 2、usb 1-4-port1:cannote enable. maybe the usb cable is bad？

<img src="https://silencht.oss-cn-beijing.aliyuncs.com/img/IMG_0530.JPG" alt="IMG_0530" style="zoom: 15%;" />

进入桌面后，测试板子上所有USB口都能正常工作。排除板子硬件问题后，发现USBHUB坏了，换个新的就警告了。

<img src="https://silencht.oss-cn-beijing.aliyuncs.com/img/5D467BEA4EF6CA3C9B0F0F5AA1DBCD42.png" alt="5D467BEA4EF6CA3C9B0F0F5AA1DBCD42" style="zoom:15%;" />

#### 3、cp: not writing through dangling symlink 'etc/resolv.conf'

etc/resolv.conf 是记录DNS服务参数的文件，手动修改8.8.4.4，114.114.114.114等DNS参数均无法解决。

#### 4、cgroup：cgroup2：unknown option “nsdelegate”

论坛说可能是系统内核版本与系统版本相差过大导致的问题，解决方法暂时不明。

#### 4、联想显示器无法成功启动问题

通过HDMI-VGA线连接联想显示器无法显示画面，只能使用HDMI-HMDI显示器才可成功启动，进入桌面界面。

联想显示器显示超出输入分辨率范围，随后显示无输入信号，猜测是显示信号分辨率输出太大，通过配置 /etc/X11/xorg.conf 文件，未能解决问题。

[某论坛](https://www.taodudu.cc/news/show-627002.html)称：“ **坑1**：请使用HDMI接口的显示器或者HDMI转DVI，HDMI转VGA无法通过开机自检，开机后可以更换HDMI转VGA接头显示成功。”

解决方法：

- HDMI-HDMI接口显示器（成功）
- VNC或SSH方式（暂未尝试）

#### 5、英伟达板子中文输入法
搜狗输入法只支持amd架构，而英伟达板子是arm架构，因此搜狗输入法很难安装。谷歌拼音输入法可以安装，但是xavier板子上有不会出现字的候选框的bug，知乎某答主说是安装了fcitx-qimpanel-configtool的原因。所以我使用自带的ibus拼音输入法。
[原文](https://blog.csdn.net/qq_34213260/article/details/106226831)
1. 安装文件 ibus-pinyin

   ibus是系统已经安装，直接输入ibus会显示如下内容：

   ```
   ibus
   ```

   用法：ibus 命令 [选项...]
   命令：
     engine          设定或获取引擎
     exit            退出 ibus-daemon
     list-engine     显示可用引擎
     watch           (暂不可用)
     restart         重启 ibus-daemon
     version         显示版本号
     read-cache      显示注册缓存内容。
     write-cache     创建注册缓存
     address         输出 ibus-daemon 位于 D-Bus 中的地址
     read-config     显示配置值
     reset-config    重置配置
     emoji           将面板中的 emoji 保存到剪贴板 
     help            显示本信息

2. 安装ibus-pinyin

```
sudo apt-get install ibus-pinyin
```

3. 系统设置
    打开右上角系统设置，进入语言支持（language support），点击“install / remove language…”，选择简体中文，输入密码，此时系统会进行更新，大约几分钟；
    将菜单和窗口的语言栏中“汉语（中国）”拖到最上方，键盘输入法系统选择IBus，最后选择应用到整个系统按钮；

4. ibus设置
    终端下输入：

  ```
  ibus-setup
  ```

  弹出一个窗口，切换到输入法选项卡，点击添加按钮，选择汉语，选择Pinyin选项，确定；
  输入下述命令，重新启动ibus：

  ```
  ibus restart
  ```

5. 重启电脑

  ```
  reboot
  ```

6. 开机切换输入法
右上角图标栏选择文本输入设置，找到“+”号，找到汉语（pinyin）（ibus）选项，添加，确定；
切换到下一个源，使用这里是如何切换输入法，可自行选择，推荐ctrl+space
