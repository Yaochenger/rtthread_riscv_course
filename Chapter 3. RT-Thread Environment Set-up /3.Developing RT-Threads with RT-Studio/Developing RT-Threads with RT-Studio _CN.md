# RT-Studio开发RT-Thread说明

RT-Studio中支持了非常多的板级支持包(BSP)，用户可以选择自己芯片平台对应的BSP，快速的将RT-Thread在自己的平台上跑起来。

本课程的主题是RISC-V，HPM6750是一款RISC-V架构的芯片，RT-Thread对该BSP有了完善的支持，这里我们以该芯片的BSP为例，创建一个在RISC-V芯片上运行RT-Thread的样板工程。

# 安装BSP支持包

首先安装RT-Studio，[RT-Studio下载地址](https://www.rt-thread.org/download.html)。完成软件安装后，打开RT-Studio，点击菜单栏的包管理器选项。在菜单栏可以看到`SDK Manager`选项，选择该选项，

![](figures/studio_1.png)

在选项中找到`Board_Support_Package`选项，如下图所示：

![](figures/studio_2.png)

选择HPMicro，选择其中一个版本下载，通常选最新版本即可，这里勾选最新版本后点击`Install packages`选项，安装本教程所需的软件包。

![](figures/studio_3.png)

# 创建工程

安装完成后，点击`File`选项，选则`New`->`RT-Thread Project`选项来创建一个RT-Thread工程。

![](figures/studio_4.png)

下图是我们创建基于HPM6750的样板工程

![](figures/studio_5.png)

在上图中，首先需要在Board选项中，下拉找到HPM6750EVKMINI，BSP选项用于选择BSP的版本，这里选择最新版本即可，最后需要设置项目的名称，在`Project name`选项中设置工程的名称，设置完成后，点击`Finsh`选项完成工程创建。

# 编译工程

创建完成后点击下图中的红色框，进行编译，编译成功后后如下图所示：

![](figures/studio_6.png)

# 烧录固件

点击右侧的下载选项，将固件烧录到MCU中，烧录结果如下图所示：

![](figures/studio_7.png)

# 调试工程

完成工程的编译与下载后，我们可以点击下述的调试选项进入调试，调试方法与eclipse一致。

![](figures/studio_8.png)

