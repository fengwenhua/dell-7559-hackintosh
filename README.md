# 7559黑苹果
> 参考链接: [Dell 7559 安装10.14GM教程（i5+UEFI）](http://bbs.pcbeta.com/viewthread-1794485-1-1.html)
这位大佬的教程已经很完善了,只不过本人是小白,所以对这位大佬的教程进行了一些补充,如果你已经按照他的教程安装完成了,现在是10.14或者10.14.1,你需要做的就是 **升级到现在最新的10.14.3,然后跳到 0x05蓝牙不可关闭修复开始**

* 好吧,废话少说,先看看效果然后再决定往下看

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128210751.png)

1. 声卡完美,耳机自动切换,笔记本快捷键,`F1`静音,`F2`调低声音和`F3`调高声音

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128210835.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128211546.png)

2. 背光可调,也就是所谓的小太阳,笔记本快捷键`Fn+F1`和`Fn+F2`

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128211525.png)

3. USB3.0驱动成功

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128211704.png)

4. 蓝牙可用

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128211735.png)

5. 睡眠可用,电量正常显示

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128211818.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128211908.png)

6. 摄像头可用,使用mac自带的`Photo Booth`检测

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128212148.png)

7. 独显已经屏蔽

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128212230.png)

8. 可变频,测试软件[Intel Power Gadget](https://software.intel.com/en-us/articles/intel-power-gadget-20)和[cpu-s](http://bbs.pcbeta.com/viewthread-1698338-1-1.html),这两个软件我已经下载好放在附件中了

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128213353.png)


## 0x00 电脑配置
* CPU: i7-6700HQ
* 内存: DDR3L 1600MHz *2
* 硬盘: 128G+1T
* 显卡: GTX960M 4G + Intel HD Graphics 530
* 声卡: ALC256
* BIOS版本:1.2.8

## 0x01 BIOS准备
1. 如果cpu是i5的,则需要去官网下载1.0.1的BIOS版本刷入
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

1. 下载macOS Mojave镜像: [【黑果小兵】macOS Mojave 10.14.3 18D42 正式版 with Clover 4859原版镜像](https://blog.daliansky.net/macOS-Mojave-10.14.3-18D42-official-version-with-Clover-4859-original-image.html)
2. 检查下载的系统的md5是否是下载链接处提供的`450c55e5c5d3f4bfae6bb55ff2a33aea`

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128203547.png)

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

4. 在windows下,使用`Diskgenius`,mac下使用`Clover Configurator`挂载U盘的EFI分区,然后将附件中的`CLOVER`文件夹**替换**u盘`EFI`文件夹中的`CLOVER`,

> ps:如果你的配置以及Bios版本和我的一致,先删除U盘中的`CLOVERE`,然后你可以直接将附件中的`最终的CLOVER`重命名成CLOVER,然后放到U盘的EFI分区里面

* 使用`Diskgenius`替换`CLOVER`: 这里记得一点,删除原来的`CLOVER`之后,直接将附件的`CLOVER`拖动进去是没用的,需要用快捷键`CTRL+C`和`CTRL+V`

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128202335.png)

* 使用`Clover Configurator`分别挂载U盘和本地的EFI

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128204039.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128204301.png)


5. 然后可以拿着这个U盘启动盘去装系统了,具体的如何安装系统以及将启动U盘的CLOVER放在本地的EFI下的步骤这里略过,如果上面的操作正常,安装完成后是可以进入到mac系统的

## 0x03 驱动安装
1. 移除驱动，终端输入：

```
sudo rm -rf /System/Library/Extensions/AppleACPIPS2Nub.kext
```

2. 将附件中的驱动使用`KCMP Utility Pro`安装至`L/E`分区,非常简单,将这些驱动全部拖进去即可安装

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128204359.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128204451.png)

3. 重启准备进行`dsdt`和`ssdt`制作

## 0x04 DSDT和SSDT文件制作
> 如果你对BIOS版本和我的一样,那么直接用我`CLOVER/ACPI/patched`文件夹里面的文件即可,也就是说不用看这节的内容了.需要说明的是,一般来说,不同BIOS版本的文件不能通用，容易出错

#### 前期准备
1. 开机选择Mac图标,按下`F4`，所需文件将会出现在`EFI/Clover/ACPI/origin`文件夹
2. 使用`Clover Configurator`加载引导分区EFI,如下图

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220355.png)

3. 将`EFI/Clover/ACPI/`下的`origin`文件夹复制到桌面

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220418.png)


4. 复制附件中的`iasl`和`refs`文件至桌面的`origin`文件夹
5. 终端输入:

```
cd ~/Desktop/origin
sudo cp iasl /usr/bin
iasl -da -dl -fe refs.txt DSDT.aml SSDT*.aml
```
 将会有`****.dsl`文件创建于`origin`文件夹,如下图

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220427.png)

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

![](_v_images/20190127214617502_1679233775.png)

#### SSDT-5-SaSsdt.aml制作
1. 打开`SSDT-5-SaSsdt.dsl`，先点`Compile`,没有error之后,再点击`patch`，选择`[igpu] Rename GFX0 to IGPU`  然后点击`Apply`,同理另存为`ACPI Machine Language Binary`格式保存至**桌面,**如下两个图

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220855.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220918.png)

2. 拷贝制作的`SSDT-5-SaSsdt.aml`文件和附件中`SSDT-PNLF.aml`、`SSDT-UIAC.aml`至引导分区`EFI/Clover/ACPI/patched`

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

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220929.png)

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

4. 切换到`Devices`标签,Audio下的Inject选择13

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127221357.png)

4. 重启

## 0x07 触摸板软点击(可选)
> 进行了这步操作,触摸板可进行软点击,但是多指手势就没了

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

## 0x08 10.14.1升级10.14.3的背光修复
> 如果你是按照我的教程来重新安装10.14.3,那么不用看下面,如果你是安装我参照的教程安装的10.14或10.14.1,则需要执行下方教程

1. 升级完成后,发现声音没了,背光无法调节
2. 下载亮度调节驱动下载: https://bitbucket.org/RehabMan/applebacklightfixup/downloads/
3. 通过`KCPM Utility Pro`将`AppleBacklightFixup.kext`装进`/L/E/`下
4. 将`EFI/EFI/CLOVER/ACPI/patched`下的,原来`的SSDT-PNLF.aml`和`SSDT-PNLF-AppleBacklight.aml`删掉
5. 然后将下载下来的`SSDT-PNLF.aml`放在`patched`下
6. 重启即可

> 本教程的所有文件已经上传到GitHub上,如果PB币不够的话,可以上GitHub下载,[dell 7559 教程附件GitHub地址](https://github.com/fengwenhua/dell7559-hackintosh),如果PB币够的话,可以花一个PB币下载txt附件,去百度网盘下载.所以附件内容是百度云链接


