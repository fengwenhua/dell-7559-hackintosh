# 更新到10.14.6
* 直接更新, 然后按照下面的教程修复蓝牙即可
* 背光可调节, 但是小太阳不见了... ...留着之后和触摸板一起解决
* 另外, 我在github上搜索下载安装了最新的WhateverGreen, Lilu 和 AppleALC
* 最新的驱动已上传到github上

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190723115828.png)

---

# usb网卡
> 老实说, 前段时间`macOS Catalina`出了, 我也想升级一波, 但是想想, 还是等正式版出了再说吧

昨天从柜子找出一个802.11 的usb网卡, 我的笔记本现在用不了wifi(主要是穷...), 想着大佬肯定有驱动, 结果一找, 果然有, 装好后直接可以用了, usb网卡驱动地址: https://github.com/chris1111/Wireless-USB-Adapter-Clover/releases

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190612200855.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190612200909.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190612200921.png)

---

# 更新到10.14.5
* 直接更新, 然后按照下面的教程修复蓝牙即可, 其他的(如背光, 声音等)都没有问题. ~~唯一不爽的是, Karabiner不起作用...现在我的windows键与alt键没办法交换回来... ...~~(自己好了... .)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190515131242.png)

---

# 7559黑苹果10.14.4

> 参考链接: [Dell 7559 安装10.14GM教程（i5+UEFI）](http://bbs.pcbeta.com/viewthread-1794485-1-1.html)

> 安装10.14.3链接: [Dell-7559-10.14.3](https://fengwenhua.top/2019/02/14/dell-7559-10-14-3-黑苹果/)

* 最近出了10.14.4, 忍不住升级了一波,结果黑屏....好吧,一怒之下,直接重装,就是这么霸气!
* 好吧,废话少说,先看看效果然后再决定往下看

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171111.png)

1. 声卡完美,耳机自动切换,笔记本快捷键,`F1`静音,`F2`调低声音和`F3`调高声音

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171125.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128211546.png)

2. 背光可调,也就是所谓的小太阳,笔记本快捷键`Fn+F1`和`Fn+F2`

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128211525.png)

3. USB3.0驱动成功

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171135.png)

4. 蓝牙可用,可关闭打开

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171149.png)

5. 睡眠可用,电量正常显示

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171224.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128211908.png)

6. 摄像头可用,使用mac自带的`Photo Booth`检测

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171234.png)

7. 独显已经屏蔽

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171243.png)

8. 可变频,测试软件[Intel Power Gadget](https://software.intel.com/en-us/articles/intel-power-gadget-20)和[cpu-s](http://bbs.pcbeta.com/viewthread-1698338-1-1.html),这两个软件我已经下载好放在附件中了

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171253.png)


## 0x00 电脑配置
* CPU: i7-6700HQ
* 内存: DDR3L 1600MHz *2
* 硬盘: 128G+1T
* 显卡: GTX960M 4G + Intel HD Graphics 530
* 声卡: ALC256
* BIOS版本:1.2.8

## 0x01 BIOS准备
1. 如果cpu是i5的,则需要去官网下载1.0.1的BIOS版本刷入,我是i7,所以不用管
2. 更改BIOS设置如下

```
 - 恢复BIOS默认设置
 - AHCI mode 选择 SATA
 - 关闭 VT for direct I/O
 - 关闭 Advanced Battery Charging
 - 关闭 Secure boot
 - 关闭 SupportAssist System Resolution
 - 关闭 Firmware TPM
```

## 0x02 系统安装

1. 下载macOS Mojave镜像: [黑果小兵】macOS Mojave 10.14.4 18E226 正式版 with Clover 4903原版镜像](https://blog.daliansky.net/macOS-Mojave-10.14.4-18E226-official-version-with-Clover-4903-original-image.html)
2. 检查下载的系统的md5是否是下载链接处提供的`ee923768b29194efc704bcf34d7f9fd8`

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171306.png)


* windows下,使用`certutil`命令

```
certutil -hashfile 系统镜像 MD5
```

* mac下使用`md5`

```
md5 系统镜像
```


3. 如果在windows下,则使用`transmac`将镜像写入U盘,如果在mac上,则使用`balenaEtcher`将镜像写入U盘

* 使用`transmac`将镜像写入U盘:先选择`Format Disk for Mac`格式化为Mac格式,然后选择`Restore with Disk Image`将镜像写入U盘

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128202859.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128203110.png)

* 使用`balenaEtcher`将镜像写入U盘:傻瓜式操作,选择下载的系统镜像,选择U盘,然后点击`Flash`,等待即可

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128203143.png)

4. 在windows下,使用`Diskgenius`,mac下使用`Clover Configurator`挂载U盘的EFI分区,我们需要修改一下`config.plist`,将原来默认的`config.plist`重命名成`config_bak.plist`,然后找到有`530`字眼的config文件,将其重命名为`config.plist`

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171319.png)

> ps:如果你的配置以及Bios版本和我的一致,先删除U盘中的`CLOVERE`,然后你可以直接将附件中的`最终的CLOVER`重命名成`CLOVER`,然后放到U盘的EFI分区里面,当然,不能保证可以进入系统!最好还是跟我教程,自己一步步安装!

5. 然后可以拿着这个U盘启动盘去装系统了,具体的**如何安装系统以及将启动U盘的CLOVER放在本地的EFI下**的步骤这里略过,如果上面的操作正常,安装完成后是可以进入到mac系统的

## 0x03 驱动安装
1. `Clover Configurator`挂载本地`EFI`,进入`/EFI/CLOVER/kexts/Other`,移除多余的驱动,以及增加一些必备的驱动.

原来的如下:

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171331.png)

修改后如下:  下面增加的驱动,都可以在下面`Backup`文件夹找到, 如果找不到,就去`最终的clover`那里找

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171341.png)

2. 让`Other`下的驱动可以生效, 打开`/EFI/CLOVER/config.plist`,在`System Parameters->Inject Kexts`改成`Yes`,然后保存

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171350.png)

3. 移除键鼠标驱动, 防止与`Other`下的鼠标驱动`ApplePS2SmartTouchPad.kext`冲突, 终端输入：

```
sudo rm -rf /System/Library/Extensions/AppleACPIPS2Nub.kext
```

4. 安装`Clover_v2.4k_r4901.pkg`,这个很重要,需要自定义安装`RC Scripts`,不然可能无法保存背光

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171402.png)

5. 重启准备进行`dsdt`和`ssdt`制作

## 0x04 DSDT和SSDT文件制作
> 如果你对BIOS版本和我的一样,那么直接用我`CLOVER/ACPI/patched`文件夹里面的文件即可,也就是说不用看这节的内容了.需要说明的是,一般来说,不同BIOS版本的文件不能通用，容易出错, 从而进不去系统

#### 前期准备
1. 开机选择Mac图标,按下`F4`，所需文件会自动出现在`EFI/Clover/ACPI/origin`文件夹
2. 使用`Clover Configurator`加载引导分区EFI,如下图

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220355.png)

3. 将`EFI/Clover/ACPI/`下的`origin`文件夹复制到桌面

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171447.png)


4. 复制附件中的`iasl`和`refs`文件至桌面的`origin`文件夹
5. 终端输入:

```
cd ~/Desktop/origin
sudo cp iasl /usr/bin
iasl -da -dl -fe refs.txt DSDT.aml SSDT*.aml
```
 将会有`****.dsl`文件创建于`origin`文件夹,如下图

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171500.png)

#### DSDT文件制作
> 注意每改一下,都要`Compile`一下,查看是否有error,修复它,不管warning

1. 使用`MaciASL.app`打开`DSDT.dsl`文件
2. 点击`Compile`,出现4个error,都是`PARSEOP_EXP_MULTIPLY`错误,双击跳转到error位置,删掉这四行即可,如果遇到其他错误自行百度

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220439.png)

3. 查找`HECI` 用`IMEI`替代;查找`HDAS` 用`HDEF`替代,

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220532.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220541.png)

然后再`Compile`一下,不用管`Warnings`,没有`Errors`即可

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220603.png)

3. 点击`Patch`,在`RehabMan Laptop`下,查找到以下项目，单击后按`Apply`

```
-[syn] Rename _DSM methods to XDSM
-[Audio] Audio Layout 3
-[bat] Dell Inspiron 15-7xxx
-[sys] Fix _WAK Arg0 v2
-[sys] Fix Mutex with non-zero SyncLevel
-[sys] HPET Fix
-[sys] IRQ fix
-[sys] RTC Fix
-[sys] Skylake LPC
-[sys] SMBUS Fix
-[usb] USB3_PRW 0x6D Skylake (instant wake)
```

* 下面以`[syn] Rename _DSM methods to XDSM`为例,点击`Patch`,选择`_RehabMan Laptop`(这个名字是自己取的,只要Patch源是  http://raw.github.com/RehabMan/Laptop-DSDT-Patch/master 即可)

> 我放在附件中的`MaciASL`是最新版本的,已经包含了`RehabMan Laptop`的源,如下图.如果你发现点击`Patch`之后**显示空白**,则说明网络不好,要么等一会,要么科学上网.

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128185836.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220635.png)

* 选择`[syn] Rename _DSM methods to XDSM`之后,等一会,然后点击`Apply`,然后再等一会,指导右边上下两个框框都没有东西了,说明替换成功
> ps:这里多说一句,如果你是程序员的话,应该明白,这个本质就是通过正则匹配,然后替换成指定内容而已,当然这个具体为什么替换成这些内容,只有问RehabMan大佬了

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220653.png)

3. 查找`layout`，并将`3`替换为`13`

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220704.png)

4. 点击`Compile`,不管Warnings,有error就修复
5. 接下来屏蔽独显： 开头处添加

```
External (_SB_.PCI0.PEG0.PEGP._PS3, MethodObj)
External (_SB_.PCI0.PEG0.PEGP._PS0, MethodObj)
External (_SB_.PCI0.PEG0.PEGP._OFF, MethodObj)
External (_SB_.PCI0.PEG0.PEGP._ON, MethodObj)
External (_SB_.PCI0.PEG0.PEGP.SGOF, MethodObj)
External (_SB_.PCI0.PEG0.PEGP.SGON, MethodObj)
```

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220725.png)

搜索`_WAK`，并在其之前添加

```
Method (M_ON, 0, NotSerialized)
    {
        If (CondRefOf (\_SB_.PCI0.PEG0.PEGP._ON))
        {
            \_SB_.PCI0.PEG0.PEGP._ON()
        }
        If (CondRefOf (\_SB_.PCI0.PEG0.PEGP._PS0))
        {
            \_SB_.PCI0.PEG0.PEGP._PS0()
        }
        If (CondRefOf (\_SB_.PCI0.PEG0.PEGP.SGON))
        {
            \_SB_.PCI0.PEG0.PEGP.SGON()
        }
    }

Method (M_OF, 0, NotSerialized)
    {
        If (CondRefOf (\_SB_.PCI0.PEG0.PEGP._OFF))
        {
            \_SB_.PCI0.PEG0.PEGP._OFF()
        }
        If (CondRefOf (\_SB_.PCI0.PEG0.PEGP._PS3))
        {
            \_SB_.PCI0.PEG0.PEGP._PS3()
        }
        If (CondRefOf (\_SB_.PCI0.PEG0.PEGP.SGOF))
        {
            \_SB_.PCI0.PEG0.PEGP.SGOF()
        }
    }
```

如下图

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220755.png)

搜索`_WAK`，添加`M_OF()`,如下图

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220811.png)

搜索`_PTS`，添加`M_ON()`,如下图

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220833.png)

搜索`Method (_INI, 0, Serialized)`，并在图示位置添加`M_OF()`

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220845.png)

6. 点击`Compile`，检查有无错误，无错误则选择`File`->`Save as...`->`ACPI Machine Language Binary`格式保存至桌面，有错误则从头再来

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171516.png)

7. 拷贝制作的`DSDT.aml`文件至引导分区`EFI/Clover/ACPI/patched`

#### SSDT-5.aml制作
1. 打开`SSDT-5.dsl`，先点`Compile`,没有error之后,再点击`patch`，选择`[igpu] Rename GFX0 to IGPU`  然后点击`Apply`,同理另存为`ACPI Machine Language Binary`格式保存至**桌面,**如下两个图

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171523.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220918.png)

2. 拷贝制作的`SSDT-5.aml`文件和附件中`SSDT-PNLF.aml`、`SSDT-UIAC.aml`至引导分区`EFI/Clover/ACPI/patched`

#### 制作SSDT.aml
* 打开终端,输入下方两条指令

```
curl -o ssdtPRGen.sh https://raw.githubusercontent.com/Piker-Alpha/ssdtPRGen.sh/Beta/ssdtPRGen.sh
chmod +x ssdtPRGen.sh
```

* 如果是i7 6700HQ使用

```
./ssdtPRGen.sh -p 'i7-6700HQ' -f 2600 -turbo 3500
```

* 如果是i5 6300HQ使用

```
./ssdtPRGen.sh -p 'i5-6300HQ' -f 2300 -turbo 3200
```

根据提示选择`y`、`n`

```
cp /Users/用户名/Library/ssdtPRGen/ssdt.aml /Users/用户名/Desktop/SSDT.aml
```

同理把`SSDT.aml`拷贝至引导分区`EFI/Clover/ACPI/patched` 至此你的`patched`文件夹应包含以下内容

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171537.png)

然后重启即可

## 0x05 蓝牙不可关闭修复
> 总结起来就是,根据供应商ID,找到产品ID,然后正确修改产品ID

1. `关于本机`->`系统报告`->找到蓝牙的供应商ID和产品ID

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127155708.png)

```
供应商 ID：   0x8087
产品 ID：    0x0A2A
```

2. 转换为10进制

```
供应商 ID：    32903
产　品 ID：    2602
```

3. 编辑`/System/Library/Extensions/IOBluetoothFamily.kext/Contents/PlugIns/BroadcomBluetoothHostControllerUSBTransport.kext/Contents/Info.plist`文件，搜索 `IOKitPersonalities`,然后修改`idVendor`值为`32903`和`idProduct`的值为`2602`如下：
原来的如下

```xml
	<key>IOKitPersonalities</key>
	<dict>
		<key>Broadcom2045FamilyUSBBluetoothHCIController_D</key>
		<dict>
			<key>CFBundleIdentifier</key>
			<string>com.apple.iokit.BroadcomBluetoothHostControllerUSBTransport</string>
			<key>IOClass</key>
			<string>BroadcomBluetoothHostControllerUSBTransport</string>
			<key>IOProviderClass</key>
			<string>IOUSBHostDevice</string>
			<key>idProduct</key>
           <integer>33292</integer>
			<key>idVendor</key>
			<integer>1452</integer>
```

修改后如下

```xml
	<key>IOKitPersonalities</key>
	<dict>
		<key>Broadcom2045FamilyUSBBluetoothHCIController_D</key>
		<dict>
			<key>CFBundleIdentifier</key>
			<string>com.apple.iokit.BroadcomBluetoothHostControllerUSBTransport</string>
			<key>IOClass</key>
			<string>BroadcomBluetoothHostControllerUSBTransport</string>
			<key>IOProviderClass</key>
			<string>IOUSBHostDevice</string>
			<key>idProduct</key>
			<integer>2602</integer>
			<key>idVendor</key>
			<integer>32903</integer>
		</dict>
```

4. 使用`Kext Utility`修复权限和重建缓存即可

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127155800.png)

5. 效果如下,可开启,可关闭

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127224543.png)


## 0x06 耳机爆音修复
1. 进入附件的`ALCPlugFix`目录
2. 双击`install双击自动安装.command`
3. 用`Clover Configurator`打开`EFI/Clover/config.plist`,切换到`Boot`,在`Custom Flags`添加`-lilubetaall -alcbeta`

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127221156.png)

4. 切换到`Devices`标签,Audio下的`Inject`选择`13`
> 其实这一步操不操作没关系,因为我们已经在DSDT.aml中指定了

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127221357.png)

4. 重启

## 0x07 触摸板
> `ApplePS2SmartTouchPad.kext`的作者已经不更新多年了, 本人装了这个驱动, 虽然触摸板上可以看到手势,但是却一个也用不了... ...所以下面是讲怎么安装`RehabMan`的触摸板驱动`VoodooPS2Controller.kext`的, 当然,如果你嫌麻烦,直接下载`VoodooPS2Controller.kext`放到`Other`下,然后重启一波就行!

1. 安装指导链接 https://github.com/RehabMan/OS-X-Voodoo-PS2-Controller/wiki/How-to-Install
2. 软件下载链接 https://bitbucket.org/RehabMan/os-x-voodoo-ps2-controller/downloads/
3. 从`/S/L/E`和`/L/E`中删除不兼容的kexts：
```
sudo rm -rf /System/Library/Extensions/AppleACPIPS2Nub.kext
sudo rm -rf /System/Library/Extensions/ApplePS2Controller.kext
sudo rm -rf /System/Library/Extensions/ApplePS2SmartTouchPad.kext
sudo rm -rf /Library/Extensions/AppleACPIPS2Nub.kext
sudo rm -rf /Library/Extensions/ApplePS2Controller.kext
sudo rm -rf /Library/Extensions/ApplePS2SmartTouchPad.kext
```
4. 安装VoodooPS2Controller.kext（10.11及更高版本）：
```
cd directory-to-where-your-unzipped-download-is
sudo cp -R Release/VoodooPS2Controller.kext /Library/Extensions
```
5. 重建内核缓存：
```
sudo touch /System/Library/Extensions && sudo kextcache -u /
```

6. 然后重启系统。
7. 如果发现笔记本自带键盘`windows`键和`alt`键的功能交换了,则需要用到`Karabiner`交换`windows`和`alt`的功能

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128211336.png)

## 0x08 背光保存
* 删除`/EFI/CLOVER/drivers64UEFI/EmuVariableUefi-64.efi`,然后重启两遍以上,即可保存背光亮度

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171552.png)

## 0x09 其他的驱动
* 将`其他的驱动`里面的驱动拖到`/EFI/CLOVER/kexts/Other`里面

驱动名|驱动内容
--|--
Sinetek-rtsx.kext|SD卡驱动,没测试过,不知道有没有用
BrcmFirmwareData.kext和BrcmNonPatchRAM2.kext和BrcmPatchRAM2.kext|蓝牙驱动
FakePCIID_Broadcom_WiFi.kext|无线网卡驱动
FakePCIID_Intel_HD_Graphics.kext|内置显卡驱动
FakePCIID_Intel_HDMI_Audio.kext|HDMI_Audio驱动
FakePCIID.kext|没有这个,上面几个FakePCIID_xxx都无法正常工作
Sinetek-rtsx.kext|SD卡驱动,没测试过,不知道有没有用没哟
Sinetek-rtsx.kext|SD卡驱动,没测试过,不知道有没有用

* 这个给出`蓝牙驱动`和`FakePCIID系列`的下载地址

```
https://bitbucket.org/RehabMan/os-x-brcmpatchram/downloads/
https://bitbucket.org/RehabMan/os-x-fake-pci-id/downloads/
```

* 关于驱动的说明: http://bbs.pcbeta.com/viewthread-1779539-1-1.html

## 最后
* 欢迎大家访问我的博客: https://fengwenhua.top , 虽然博客上面没啥东西!





