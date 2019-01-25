原链接 [https://www.tonymacx86.com/threads/an-idiots-guide-to-lilu-and-its-plug-ins.260063/](https://www.tonymacx86.com/threads/an-idiots-guide-to-lilu-and-its-plug-ins.260063/)

![e8a5491ce611d3b05a8299e68098c6c6.png](https://www.tonymacx86.com/attachments/lilupluginguide-png.350539/)
## Lilu及其插件傻瓜教程
上次更新：2019.01.17: FB-Patcher重命名为Hackintool  

**注意**：你需要使用Lilu与它的插件的**最新版本**

-----
### 关于此教程
该教程写给
1. 正在使用Lilu并准备迁移到Mojave的用户
2. 使用传统patch方法想要更换为Lilu的patch方法

我需要声明我与Lilu没有任何直接关系, 我第一次遇见Lilu是2017年，我当时需要解决笔记本的4k屏幕的刷新率问题，从那之后我一直关注着Lilu的开发进程，我现在认为Lilu是整个黑苹果社区工具里面最有用的之一。

-----
### Lilu是什么

Lilu是一个MacOS的补丁引擎("Patching Engine ")，它允许对内核和系统插件进行动态更新(on-the-fly patching)，Lilu提供了一种比传统黑苹果方式好得多的方法，包括：
- 不依赖启动引导(bootloader)
- 可以在恢复模式和MacOS安装时候使用
- 支持一种使用符号化(symbolic)的补丁引擎
- 提供API访问
- 允许运行时修改

Lilu和Clover一样,都是经过了很多年的更新,成了主流选择。只有Lilu是没有用的，它必须配合插件进行使用。

Lilu有自己的SDK，任何有能力的人都可以开发它的插件，因此它获得了大量开发者和用户的追随，也因此他成为了"补丁引擎"（Patching Engine）的最优选择

本文讨论Lilu插件里最流行的3个:
- [WhateverGreen](https://github.com/acidanthera/WhateverGreen) 提供GPU支持，包括Intel/AMD/Nvidia
- [AppleALC](https://github.com/acidanthera/AppleALC) AppleHDA的动态补丁(Dynamic patching)，主要用于非官方支持声卡的驱动
- [AirportBrcmFixup](https://github.com/acidanthera/AirportBrcmFixup) 各种各样的博通无线网卡补丁

这些插件将会在下面详细介绍，如果你想要了解更多Lilu的可用插件，可以访问[Lilu已知插件](https://github.com/acidanthera/Lilu/blob/master/KnownPlugins.md)

**重要提示**：以下所有的Lilu插件都自带了完整的解决方案，它将替换掉相关的Clover Patch和kexts，因此你在使用前需要移除你已经安装的相关patch和kexts， 例如你要使用AppleALC插件，你需要先移除AppleHDA相关的patch，kexts，HDA Enabler，在未移除情况下使用插件将导致不可预估的结果

每个插件都有它自己的文档，你可以访问它的Git-Hub链接，查看里面的readme.md文件或者wiki页面，以获得更多的帮助。

Lilu及其插件可以在相关Git-Hub链接里面点击**Releases**来获取，你可以查看到更多版本历史，大多数用户只需要下载最新的**Release**版本即可，少数情况下你需要下载**Debug**版本或者更早的版本，更少的情况下你需要自己下载源代码进行编译。

Lilu及其插件只能使用在黑苹果系统中，需要是Sierra+(10.12+)的系统版本，推荐在Mojave(10.14)里面使用

-----
### 如何安装
Lilu及其插件都是kext，都需要安装到/Library/Extensions文件夹中。你可能了解到一些人把所有的东西都放在Clover/Kexts/Other文件夹里面，但是这种方式应该只用于开发和测试的目的。安装到/Library/Extensions文件夹能够确保它们可能被系统正确的构建缓存，这应该是所有的第三方kext(包括FakeSMC)的首选，如果你要了解为什么安装到/Library/Extensions而不是Clover更多的相关解释，请访问[Installing 3rd Party Kexts - El Capitan, Sierra, High Sierra, Mojave +](https://www.tonymacx86.com/threads/guide-installing-3rd-party-kexts-el-capitan-sierra-high-sierra-mojave.268964/)

**注意**：你需要把FakeSMC也安装在Clover/Kexts/Other文件夹来注入到clover，因为你可能需要引导安装或者进行MacOS恢复(MacOS recovery)，这是唯一一个你真的需要保留在Clover/Kexts/Other里面的kext，其他的都不需要

你可以在下面的链接中下载Lilu和3个需要使用到的插件
- [Lilu](https://github.com/acidanthera/Lilu)
- [WhateverGreen](https://github.com/acidanthera/WhateverGreen) 提供GPU支持，包括Intel/AMD/Nvidia
- [AppleALC](https://github.com/acidanthera/AppleALC) AppleHDA的动态补丁(Dynamic patching)，主要用于非官方支持声卡的驱动
- [AirportBrcmFixup](https://github.com/acidanthera/AirportBrcmFixup) 各种各样的博通无线网卡补丁

上面每一个链接都会跳转到Git-Hub页面，点击**Releases**，然后下载最新的版本，解压，拷贝到某个地方

如果你对命令行复制kext到/Library/Extensions并重新构建缓存不熟悉，你可以使用下面的方式，如果熟悉可以跳过

下载[KextBeast](https://www.tonymacx86.com/resources/kextbeast-2-0-2.399/)，拷贝所有需要安装的kext到桌面，然后运行KextBeast，点击继续几次然后当提示输入密码时候输入密码，选中 /Library/Extensions然后点击继续
![pic](https://tencent-1256324020.cos.ap-shanghai.myqcloud.com/Lilu%20Guide/Screen%20Shot%202018-09-09%20at%2018.03.50.png)

KextBeast会把你放在桌面上的kext安装到你选中的文件夹里面，在KextBeast完成后将会提示你是否删除KextBeast，我通常点击保留，我总会尝试新版本的kext，KextBeast能够很好的完成该工作。

一旦KextBeast完成后，我将运行cVad的Kext Utility 2.6.6[点击下载](http://cvad-mac.narod.ru/files/Kext_Utility.app.v2.6.6.zip)，这个程序将会修复kext的权限问题而且会重建缓存，你只需要输入密码，等待它完成工作。如果你看到了退出(Quit)按钮的字体变为黑色，说明它的工作已经完成。
![Kext Utility](https://tencent-1256324020.cos.ap-shanghai.myqcloud.com/Lilu%20Guide/Kext%20Utility.png)

**注意** 不要使用Kext Utility去安装Lilu和它的插件，这个软件并不支持安装kext到 /Library/Extensions，它应该只被用于修复kext的权限问题和重新构建缓存。

Kext Utility运行完成后，重启系统。

-----
### WhatEverGreen插件

WhatEverGreen的第一个版本只支持AMD的GPU，Nvidia和Intel GPU需要使用Lilu的其他特定的插件进行支持，但是在2018年年中的时候，WhatEverGreen将原本的AMD GPU的支持和下列的所有插件合并了：
- [IntelGraphicsDVMTFixup](https://github.com/BarbaraPalvin/IntelGraphicsDVMTFixup) Intel核显DVMT Pre-allocation补丁
- [IntelGraphicsFixup](https://github.com/lvs1974/IntelGraphicsFixup) 用来支持Intel的核显
- [NvidiaGraphicsFixup](https://github.com/lvs1974/NvidiaGraphicsFixup) 用来支持Nvidia的核显
- [CoreDisplayFixup](https://github.com/PMheart/CoreDisplayFixup) 用来支持 HDPI的补丁(pixel clock patch)
- [Shiki](https://github.com/acidanthera/Shiki) 用于有DRM保护的视频播放
- [AzulPatcher4600](https://github.com/coderobe/AzulPatcher4600) 给HD4600核显的Azul framebuffer特定补丁

WhatEverGreen(WEG)现在是黑苹果GPU和显示的一站式解决方案

**重要** 2018年7月后上面6个插件都已经包含到WhatEverGreen，原项目都已废弃而且不再更新，如果上述插件结合WhatEverGreen和最新的Lilu使用，会导致意想不到的后果

对于AMD和Nvidia的使用者，WhatEverGreen包含一系列的修复和补丁，比如启动时黑屏之类的，只需要简单安装WhatEverGreen一样可以使独立显卡工作，更有经验的用户可以在WhatEverGreen的readme.md和手册里面查看AMD和Nvidia的显卡启动参数。

对于核显或者核显+独显的系统，WhatEverGreen提供了一种比以前使用Clover或者自行定制Intel framebuffer kext更加简单的方式

**重要：**如果你有一个集显+独显的笔记本，你可能需要先关闭独显来进行下一步，你可以查看[RehabMans guide](https://www.tonymacx86.com/threads/guide-disabling-discrete-graphics-in-dual-gpu-laptops.163772/)查看更多

**准备**

WhatEverGreen将会处理你的ACPI重命名，因此如果你进行过下列ACPI重命名，你需要先移除它们

- 改变/重命名 GFX0 到 IGPU
- 改变/重命名 PEG0 到 GFX0
- 改变/重命名 HECI 到 IMEI

上述使用一般方法的ACPI的更改可能会在之后造成问题。
