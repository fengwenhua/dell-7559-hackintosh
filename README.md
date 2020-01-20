# 2020.01.20 解决双显黑屏
> 带着我的笔记本高高兴兴的回家, 结果发现, 插入显示器的时候, 笔记本内屏是黑屏的, 外接显示器正常, 于是赶紧上远景找一波解决方案, 原文链接: http://bbs.pcbeta.com/forum.php?mod=viewthread&tid=1834012&highlight=hdmi

1. 进入`/System/Library/Extensions`目录，将`AppleGraphicsControl.kext` 拷贝到桌面。

    ```
    cd /System/Library/Extensions && cp -R AppleGraphicsControl.kext ~/Desktop
    ```

2. 使用 `Clover Configurator` 挂载EFI, 打开 `config.plist`, 进入`SMBIOS`栏, 复制`Board-ID`

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1579501469_20200120141624298_457252686.png)

3. 用文本编辑器(比如我用sublime) 打开桌面的`AppleGraphicsControl.kext/Contents/PlugIns/AppleGraphicsDevicePolicy.kext/Contents/Info.plist`
4. 搜索关键字`ConfigMap`, 在下面插入如下代码:

    ```
    <key>你的Board-ID</key>
    <string>none</string>
    ```

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1579501472_20200120141814828_1349404636.png)

5. 修改完成后保存关闭. 然后依次输入以下命令解锁`S/L/E`权限

    ```
    sudo su
    sudo mount -uw /
    killall Finder
    ```

6. 打开`Kext Utility` 将改好的 `AppleGraphicsControl.kext` 拖进去, 写入驱动目录.

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1579501476_20200120142049032_2016404932.png)

7. 重启, 问题解决.

---

# 2019.12.11 更新10.15.2
更新完后, 修复声音即可, 以下是步骤:

> 下载替换如下驱动的最新版:

* [Clover](https://github.com/Dids/clover-builder/releases) 下载`5100`版本的`CLOVERX64.efi`
* [WhateverGreen_v1.3.5](https://github.com/acidanthera/WhateverGreen/releases)
* [Lilu_v1.4.0](https://github.com/acidanthera/Lilu/releases)
* [AppleALC_v1.4.4](https://github.com/acidanthera/AppleALC/releases)

然后直接更新

更新完后, 没有声音, 先解锁SLE权限

```
sudo su
sudo mount -uw /
killall Finder
```

然后用附件的`耳机爆音修复`, 双击`install双击自动安装.command`, 修复完成后重启电脑即可

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1576033673_20191211110352592_522659038.png)

---

# 2019.11.9 更新10.15.1
直接在系统设置那里更新

更新完成之后, 发现声音没了, 蓝牙, wifi, 小太阳正常

修复声音: 更新lilu applealc whatevergreen到最新版本即可

# 2019.11.1 更换网卡1820A
最近拿到了心目中的offer, 感觉飘飘然, 于是乎, 某宝花了70大洋, 干了一个1820A, 以下就是安装教程

## 注意
如果你系统还没安装好, 就直接插上1820A网卡, 可能会一直卡住, 无法进入安装界面... 所以只能拔卡了~~~

如果你已经装好系统Catalina了, 更换成1820A网卡, 你可能会遇到下面两种错误

### Begin Gfx firmware load process 错误
```
在启动参数里加 -disablegfxfirmware 就好了
```

![](https://raw.githubusercontent.com/fengwenhua/api-test/master/1573036545_20191020085411554_977482903.png)


### appleintellpssi2ccontroller ftimerservicematching timed out 错误

删掉`/System/Library/Extensions/`下的`AppleIntelLpssI2C.kext` 和 `AppleIntelLpssI2CController.kext`

## 我的安装步骤
1. 安装10.14.5, 进入系统
2. 替换`clover.efi`为最新, 替换`whatevergreen`, `lilu`, `AppleALC`为最新
3. 删掉`/System/Library/Extensions/`下的`AppleIntelLpssI2C.kext` 和 `AppleIntelLpssI2CController.kext`
4. 在启动参数里加 `-disablegfxfirmware`
5. 重启, 进入系统设置更新catalina
6. 更新完后, 如果还遇到上面提到的第二种错误, 则拔卡, 正常进入Catalina, 再删一遍`/System/Library/Extensions/`下的`AppleIntelLpssI2C.kext` 和 `AppleIntelLpssI2CController.kext`, 再插卡进入Catalina
7. 接下来就是安装wifi和蓝牙驱动

## 理论安装步骤
> 我根据我的安装步骤, 推导出来的, 还未实践过

1. 拔掉 7559 自带wifi网卡
2. 安装10.14.5, 进入系统
3. 替换`clover.efi`为最新, 替换`whatevergreen`, `lilu`, `AppleALC`为最新
4. 在启动参数里加 `-disablegfxfirmware`
5. 进入系统设置更新catalina
6. 更新完成之后, 先正常进入Catalina, 删掉`/System/Library/Extensions/`下的`AppleIntelLpssI2C.kext` 和 `AppleIntelLpssI2CController.kext`
7. 关机, 插上1820A网卡
8. 正常进入Catalina
9. 安装驱动

## 安装wifi驱动
成功的插着1820A网卡, 进入到Catalina之后, 就是安装驱动了

教程链接: https://blog.daliansky.net/DW1820A_BCM94350ZAE-driver-inserts-the-correct-posture.html

大致步骤如下:

1. 打开[hackintool](http://headsoft.com.au/download/mac/Hackintool.zip), 记下`PCI`->`BCM4350`中的设备地址

    ![](https://raw.githubusercontent.com/fengwenhua/api-test/master/1573036550_20191106182723159_895524893.png)

2. 将[AirportBrcmFixup](https://github.com/acidanthera/AirportBrcmFixup/releases)添加到`/EFI/CLOVER/kexts/Other`目录下
3. `config.plist` 中添加引导参数： `brcmfx-country=#a`

    ![](https://raw.githubusercontent.com/fengwenhua/api-test/master/1573036552_20191106182802892_1521478475.png)

4. `Devices`->`Properties`添加：

    - 在左侧`Devices`中添加刚刚记下来的设备地址：`PciRoot(0x0)/Pci(0x1c,0x5)/Pci(0x0,0x0)`

    - 在右侧分别添加：

        | Properties Key |          Properties Value          | Value Type |
        | -------------- | ---------------------------------- | ---------- |
        | AAPL,slot-name | WLAN                               | STRING     |
        | compatible      | pci14e4,4353                       | STRING     |
        | device_type     | Airport Extreme                    | STRING     |
        | model           | DW1820A (BCM4350) 802.11ac Wireless | STRING     |
        | name            | Airport                            | STRING     |

    ![](https://raw.githubusercontent.com/fengwenhua/api-test/master/1573036547_20191106181742558_245505540.png)

5. 重启电脑之后, wifi可用

    ![](https://raw.githubusercontent.com/fengwenhua/api-test/master/1573036554_20191106182914024_1400944496.png)

## 安装蓝牙驱动
将 [DW1820A蓝牙专用程序](http://7.daliansky.net/DW1820A/DW1820A_BT_for_Catalina_v2.5.0.zip) 解压缩到`/EFI/CLOVER/kexts/Other`目录下，重启即可

# 7559黑苹果-10.15
##  0x00 题外话
前两天看到[黑果小兵](https://blog.daliansky.net/macOS-Catalina-10.15-19A583-Release-version-with-Clover-5093-original-image-Double-EFI-Version.html)已经将Catalina的安装包搞好了, 结果下了一天也没下完... ... 昨天白天上班没空, 晚上回去终于有时间搞搞了... ...

但是因为最近感觉电脑变卡了, 我怀疑是我装了什么乱七八糟的东西导致的, 所以直接将原来的10.14.6卸载, 然后想着直接装Catalina.

结果用黑果小兵大佬的Catalina安装包, 一直卡着进不去安装... ...郁闷... ...Orz ... ...不想找bug了, 难道就没办法了吗?

不!! 恰好看到[orange1206大佬](http://bbs.pcbeta.com/forum.php?mod=viewthread&tid=1818698&highlight=7559)在六月份已经升级安装成功了!!! 直接升级安装的!!!流弊!!!!

那还等啥, 不说了, 直接去黑果小兵找10.14.5的安装包: https://blog.daliansky.net/macOS-Mojave-10.14.5-18F132-official-version-with-Clover-4928-original-image.html , 别问为什么, 我好久前就下好了, 所以直接拿来用

注意了, 因为黑果小兵最新的安装包我无法使用, 所以, **本教程是在安装10.14.5之后, 直接升级10.15, 然后再修复驱动**


> 参考链接: [Dell 7559 安装10.14GM教程（i5+UEFI）](http://bbs.pcbeta.com/viewthread-1794485-1-1.html)

> 安装10.14.6链接: [Dell-7559-10.14.6](https://blog.csdn.net/A807296772/article/details/101756415)

## 0x01 电脑配置
* CPU: i7-6700HQ
* 内存: DDR3L 1600MHz \* 2
* 硬盘: 128G+1T
* 显卡: GTX960M 4G + Intel HD Graphics 530
* 声卡: ALC256
* BIOS版本:1.2.8

> 如果配置一样或者相似, 可以继续往下看了


## 0x02 最终的效果

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1570704001.png)

1. 声卡完美, 耳机自动切换, 除了插入的时候有电流声... ... 笔记本快捷键: `F1`静音,`F2`调低声音和`F3`调高声音

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171125.png)

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128211546.png)

2. 背光可调,也就是所谓的小太阳,笔记本快捷键`Fn+F1`和`Fn+F2`

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128211525.png)

3. USB3.0驱动成功

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1570704004.png)

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

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1570704007.png)


9. iMessage可用, FaceTime没用过, 但是应该也行. **记住, 如果你想成功使用iMessage的话, 千万先别登录Apple ID**

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1570704010.png)


## 0x03 BIOS准备
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

## 0x04 系统安装

1. 下载macOS Mojave镜像: [【黑果小兵】macOS Mojave 10.14.5 18F132 正式版 with Clover 4928原版镜像](https://blog.daliansky.net/macOS-Mojave-10.14.5-18F132-official-version-with-Clover-4928-original-image.html)
2. 检查下载的系统的md5是否是下载链接处提供的`371d9145484beaf0795d6b7d0d569fc3`

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1570704013.png)

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

    > 注意: **千万别登录appleid, 或者说任何的账号, 登录与否直接影响后面的iMessage能否正常使用**

## 0x05 升级Catalina
进入10.14.5 majove之后, 先将u盘的CLOVER放到本地的EFI下, 让我们可以拔掉u盘自己启动之后, 接下来就是升级clover以升级Catalina了

然后去`https://github.com/Dids/clover-builder/releases`下载5093版本的`CLOVERX64.efi`

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1570704017.png)

然后替换本地`EFI/CLOVER`中的`CLOVERX64.efi`, 这样就完成了clover的升级了

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1570704020.png)

> [2019.11.9更新] 这里可先删掉`/System/Library/Extensions/`下的`AppleIntelLpssI2C.kext` 和 `AppleIntelLpssI2CController.kext`, 以防止后面卡住进不去系统

然后我们就可以打开`系统偏好设置`-> `软件更新`, 等一会, 它就会让我们更新Catalina, 直接下载更新, 驱动什么的等更新完Catalina之后再说

更新的过程中会重启几次, 记得选择有`Install`字样的那个选项

如果你正常操作的话, 运气不错话, 应该和我一样, 进入了Catalina系统了, 所以接下来就是驱动的安装了

## 0x06 驱动安装
1. `Clover Configurator`挂载本地`EFI`,进入`/EFI/CLOVER/kexts/Other`,移除多余的驱动,以及增加一些必备的驱动.

    原来的如下:

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171331.png)

    修改后如下:  下面修改或者增加的驱动, 去`最终的clover`那里找. 注意, 其中[AppleALC_v1.4.2](https://github.com/acidanthera/AppleALC/releases), [WhateverGreen_v1.3.3](https://github.com/acidanthera/WhateverGreen/releases), [Lilu_v1.3.8](https://github.com/acidanthera/Lilu/releases)我已经从官网升级到了最新版, 都放在了`最终的clover`上

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1570704023.png)

2. 让`Other`下的驱动可以生效, 打开`/EFI/CLOVER/config.plist`,在`System Parameters->Inject Kexts`改成`Yes`,然后保存

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171350.png)

3. 移除键鼠标驱动, 防止与`Other`下的鼠标驱动`VoodooPS2Controller_v1.9.2.kext`冲突, 终端输入：

    ```
    sudo rm -rf /System/Library/Extensions/AppleACPIPS2Nub.kext
    ```

4. 重启准备进行`dsdt`和`ssdt`制作

## 0x07 DSDT和SSDT文件制作
> 如果你对BIOS版本和我的一样,那么直接用我`CLOVER/ACPI/patched`文件夹里面的文件即可,也就是说不用看这节的内容了.需要说明的是,一般来说,不同BIOS版本的文件不能通用，容易出错, 从而进不去系统

### 前期准备
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

### DSDT文件制作
> 注意每改一下,都要`Compile`一下,查看是否有error,修复它,不管warning

1. 使用`MaciASL.app`打开`DSDT.dsl`文件
2. 点击`Compile`,出现4个error,都是`PARSEOP_EXP_MULTIPLY`错误,双击跳转到error位置,删掉这四行即可,如果遇到其他错误自行百度

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220439.png)

3. 查找`HECI` 用`IMEI`替代;查找`HDAS` 用`HDEF`替代,

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220532.png)

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220541.png)

    然后再`Compile`一下,不用管`Warnings`,没有`Errors`即可

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220603.png)

4. 点击`Patch`,在`RehabMan Laptop`下,查找到以下项目，单击后按`Apply`

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

5. 查找`layout`，并将`3`替换为`13`

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220704.png)

6. 点击`Compile`,不管Warnings,有error就修复
7. 接下来屏蔽独显： 开头处添加

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

8. 点击`Compile`，检查有无错误，无错误则选择`File`->`Save as...`->`ACPI Machine Language Binary`格式保存至桌面，有错误则从头再来

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171516.png)

9. 拷贝制作的`DSDT.aml`文件至引导分区`EFI/Clover/ACPI/patched`

### SSDT-5.aml制作
1. 打开`SSDT-5.dsl`，先点`Compile`,没有error之后,再点击`patch`，选择`[igpu] Rename GFX0 to IGPU`  然后点击`Apply`,同理另存为`ACPI Machine Language Binary`格式保存至**桌面,**如下两个图

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171523.png)

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127220918.png)

2. 拷贝制作的`SSDT-5.aml`文件和附件中`SSDT-PNLF.aml`、`SSDT-UIAC.aml`至引导分区`EFI/Clover/ACPI/patched`

### 制作SSDT.aml
打开终端,输入下方两条指令

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

## 0x08 蓝牙不可关闭修复
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

3. 打开Finder, 同时按下`windows+Shift+G`或者`Alt+Shift+G`, 在弹出的框框输入 `/System/Library/Extensions/IOBluetoothFamily.kext/Contents/PlugIns/BroadcomBluetoothHostControllerUSBTransport.kext/Contents/`, 点击`前往`

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1570704027.png)

    将`Info.plist`文件拖到桌面, 编辑它

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1570704031.png)

    搜索 `Broadcom2045FamilyUSBBluetoothHCIController_D`, 然后修改`idVendor`值为`32903`和`idProduct`的值为`2602`如下：

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

4. 接下来打开一个终端, 输入如下命令, 先解锁`S/L/E`权限, 然后再将`Info.plist`复制替换原来的文件

    ```
    sudo su
    sudo mount -uw /
    killall Finder
    ```

5. 使用`Kext Utility`修复权限和重建缓存即可

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127155800.png)

6. 效果如下,可开启,可关闭

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127224543.png)


## 0x09 耳机爆音修复
1. 进入附件的`ALCPlugFix`目录
2. 先用上面提到的解锁`S/L/E`权限的命令, 然后双击`install双击自动安装.command`
3. 用`Clover Configurator`打开`EFI/Clover/config.plist`,切换到`Boot`,在`Custom Flags`添加`-lilubetaall -alcbeta`

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127221156.png)

4. 切换到`Devices`标签,Audio下的`Inject`选择`13`
    > 其实这一步操不操作没关系,因为我们已经在DSDT.aml中指定了

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190127221357.png)

5. 重启

## 0x10 windows和alt功能交换
如果发现笔记本自带键盘`windows`键和`alt`键的功能交换了,则需要用到[Karabiner](https://pqrs.org/osx/karabiner/) 交换`windows`和`alt`的功能

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190128211336.png)

> ps: 装了这个软件之后, 发现可以用`Fn+F1`, `Fn+F2`调出小太阳了~~~

## 0x11 背光保存
* 删除`/EFI/CLOVER/drivers64UEFI/EmuVariableUefi-64.efi`,然后重启两遍以上,即可保存背光亮度

    ![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190330171552.png)

## 0x12 其他的驱动
* 将`其他的驱动`里面的驱动拖到`/EFI/CLOVER/kexts/Other`里面

    驱动名|驱动内容
    --|--
    Sinetek-rtsx.kext|SD卡驱动,没测试过,不知道有没有用
    BrcmFirmwareData.kext和BrcmNonPatchRAM2.kext和BrcmPatchRAM2.kext|蓝牙驱动
    FakePCIID_Broadcom_WiFi.kext|无线网卡驱动
    FakePCIID_Intel_HD_Graphics.kext|内置显卡驱动
    FakePCIID_Intel_HDMI_Audio.kext|HDMI_Audio驱动
    FakePCIID.kext|没有这个,上面几个FakePCIID_xxx都无法正常工作

* 这个给出`蓝牙驱动`和`FakePCIID系列`的下载地址

    ```
    https://bitbucket.org/RehabMan/os-x-brcmpatchram/downloads/
    https://bitbucket.org/RehabMan/os-x-fake-pci-id/downloads/
    ```

* 关于驱动的说明: http://bbs.pcbeta.com/viewthread-1779539-1-1.html

## 0x13 usb网卡
之前搞到一个802.11 的usb网卡, 我的笔记本现在用不了wifi(主要是穷...), 想着大佬肯定有驱动, 结果一找, 果然有, 装好后直接可以用了, usb网卡驱动地址: https://github.com/chris1111/Wireless-USB-Adapter-Clover/releases

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190612200855.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190612200909.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/20190612200921.png)

## 0x14 iMessage的修复
注意: 如果到这个时候, 你还没有登录任何账号, 那么你的成功机会应该会蛮大的~~

找到Clover的`config.plist`, 使用`Clover Configurator`打开, 选择左侧`SMBIOS`, 先勾选`Trust`, 然后使用右侧上下箭头选择适合自己的SMBIOS型号, 这里我选的是`MacBookPro13,3`, 如下图

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1570704034.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1570704037.png)

复制 `Serial Number`前往 [Apple官网查询序列号 ](https://checkcoverage.apple.com/cn/zh), 如果显示有购买日期和机型等保修信息, 说明这个序列号别人在用, 点击 `Generate New`生成新的Serial Number, 直到出现下图提示为止

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1570704042.png)

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1570704045.png)


选择 Clover Configurator 左侧 `System Parameters`, 生成一个UUID

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1570704048.png)

复制生成等UUID粘贴到`SMBIOS`的 `SmUUID` 里, 使用Win+s保存

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1570704051.png)

复制`SMBIOS`里的 `Board Serial Number`, 粘贴到Clover Configurator 左侧 `Rt Variables`的 `MLB` 里, 保存之后, 点击ROM右边的`Generate`, 确保下方三码和自己刚刚操作生成的一致, 然后保存重启即可

![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1570704055.png)



![](https://raw.githubusercontent.com/fengwenhua/ImageBed/master/1570704058.png)

重启之后, 可以打开`系统偏好设置`, 来登录自己的Apple ID了, 登录完成之后, 打开IMessage看看, 应该可以使用了~~~

> 如果不行的话, 千万憋找我~~~

## 0x15 触摸板修复
本来想好好搞一下i2c驱动起来的, 但是实在是没时间, 有能力的兄弟可以帮忙搞搞, 链接如下:
https://www.penghubingzhou.cn/2019/01/06/VoodooI2C%20DSDT%20Edit/

现在的话, 大家先把`/EFI/CLOVER/kexts/Other`下的`VoodooPS2Controller_v1.9.2.kext`换成`ApplePS2SmartTouchPad.kext`, 这样的话, 触摸板就可以简单使用了

```
单指单击相当于鼠标左键
两指滚动相当于鼠标滚轮
三指点击相当于鼠标右键
```

(这里先留个坑, 以后再搞)

## 最后
* 附件什么的, 全部已上传至 github: https://github.com/fengwenhua/dell-7559-hackintosh
* 欢迎大家访问我的博客: https://fengwenhua.top , 虽然博客上面没啥东西!


