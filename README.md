# 7559黑苹果
## 0x00 电脑配置
* CPU: i7-6700HQ
* 内存: DDR3L 1600MHz *2
* 硬盘: 128G+1T
* 显卡: GTX960M 4G + Intel HD Graphics 530
* 声卡: ALC256

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
> 参考链接: [Dell 7559 安装10.14GM教程（i5+UEFI）](http://bbs.pcbeta.com/viewthread-1794485-1-1.html)

1. 下载macOS Mojave镜像: [【黑果小兵】macOS Mojave 10.14.3 18D42 正式版 with Clover 4859原版镜像](https://blog.daliansky.net/macOS-Mojave-10.14.3-18D42-official-version-with-Clover-4859-original-image.html)
2. 检查完下载的系统的md5无误后,如果在windows下,则使用`transmac`将镜像写入U盘,如果在mac上,则使用`balenaEtcher`将镜像写入U盘
3. 在windows下,使用`Diskgenius`,mac下使用`Clover Configurator`,将附件中的`CLOVER`文件夹**替换**u盘`EFI`文件夹中的`CLOVER`
4. 然后可以拿着这个U盘启动盘去装系统了,具体的步骤这里略过,如果上面的操作正常,是可以进入到mac系统的

## 0x03 驱动安装
1. 移除驱动，终端输入：

```
sudo rm -rf /System/Library/Extensions/AppleACPIPS2Nub.kext      
```

2. 将附件中的驱动使用`KCMP Utility Pro`安装至`L/E`分区
3. 重启准备进行`dsdt`和`ssdt`制作

## 0x04 DSDT和SSDT文件制作
> 如果你对BIOS版本和我的一样,那么直接用我`CLOVER/ACPI/patched`文件夹里面的文件即可,也就是说不用看这节的内容了.需要说明的是,一般来说,不同BIOS版本的文件不能通用，容易出错

#### 前期准备
1. 开机选择Mac图标,按下F4，所需文件将会出现在`EFI/Clover/ACPI/origin`文件夹
2. 安装`MaciASL.app`和`Clover Configurator.app`,并使用`Clover Configurator`加载引导分区EFI,如下图

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
 将会有`****.dsl`文件创建于`origin`文件夹

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220427.png)

#### DSDT文件制作
> 注意每改一下,都要`Compile`一下,查看是否有error,修复它

1. 使用`MaciASL.app`打开`DSDT.dsl`文件
2. 点击`Compile`,出现4个error,都是`PARSEOP_EXP_MULTIPLY`错误,双击跳转到error位置,删掉这四行即可

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220439.png)

3. 查找`HECI` 用`IMEI`替代;查找`HDAS` 用`HDEF`替代,

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220532.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220541.png)

然后再`Compile`一下,不用管`Warnings`,没有`Errors`即可

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220603.png)

3. 点击`Patch`分别查找到以下项目，单击后按`Apply`

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

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220635.png)

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

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220755.png)

搜索`_WAK`，添加`M_OF()`

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220811.png)

搜索`_PTS`，添加`M_ON()`

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220833.png)

搜索`Method (_INI, 0, Serialized)`，并在图示位置添加`M_OF()`

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220845.png)

6. 点击`Compile`，检查有无错误，无错误则选择`File`->`Save as...`->`ACPI Machine Language Binary`格式保存至桌面，有错误则从头再来

![](_v_images/20190127214617502_1679233775.png)

#### SSDT-5-SaSsdt.aml制作
1. 打开`SSDT-5-SaSsdt.dsl`，点击patch，选择`[igpu] Rename GFX0 to IGPU` patch后Apply
同理另存为`ACPI Machine Language Binary`格式保存至桌面

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220855.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220918.png)

2. 拷贝制作的`SSDT-5-SaSsdt.aml`文件和附件中`SSDT-PNLF.aml`、`SSDT-UIAC.aml`至引导分区`EFI/Clover/ACPI/patched`

#### 制作SSDT.aml

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

## 0x05 蓝牙不可关闭
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



## 0x06 耳机爆音
1. 进入附件的`ALCPlugFix`目录
2. 双击`install双击自动安装.command`
3. 添加`-lilubetaall -alcbeta`

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127221156.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127221357.png)

4. 重启
