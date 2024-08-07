# ENV工具安装与说明

RT-Thread Env 工具包括配置器和包管理器，用来对内核和组件的功能进行配置，对组件进行自由裁剪，对线上软件包进行管理，使得系统以搭积木的方式进行构建，简单方便。

![](figures/env.png)

[ENV工具下载地址1](https://www.rt-thread.org/download.html)

[ENV工具下载地址2](https://github.com/RT-Thread/env)

# ENV工具配置

准备工作：

- 在电脑上装好 git，软件包管理功能需要 git 的支持。git 的下载地址为`https://git-scm.com/downloads`，根据向导正确安装 git，并将 git 添加到系统环境变量。
- 注意在工作环境中，所有的路径都不可以有中文字符或者空格。

使用方法：

RT-Thread 软件包环境主要以命令行控制台为主，同时以字符型界面来进行辅助，使得尽量减少修改配置文件的方式即可搭建好 RT-Thread 开发环境的方式。 

1.点击在软件包下的env.exe文件

![](figures/env_1.png)

2.右键点击菜单栏空白处，选择setting选项出现下文图的设置界面，选择Integration选项，点击Register选项将env注册到右键选项，在bsp目录下点击右键选择`ConEmu Here`选项打开env.

![](figures/env_register.png)

# ENV工具-编译

scons 是 RT-Thread 使用的编译构建工具，可以使用scons相关命令来编译RT-Thread 。

1.打开控制台后，可以在命令行模式下使用 cd 命令切换到你想要配置的 BSP 根目录中。

例如工程目录为: `rt-thread\bsp\stm32f429-apollo` ：

![](figures/env_2.png)

2.Env 中携带了 `Python & scons` 环境，只需在 `rt-thread\bsp\stm32f429-apollo` 目录中运行 `scons` 命令即可使用默认的 ARM_GCC 工具链编译 bsp，编译结果如下

![](figures/env_3.png)

如果使用 mdk/iar 来进行项目开发，可以直接使用 BSP 中的工程文件或者使用以下命令中的其中一种，重新生成工程，再进行编译下载。

```python
scons --target=iar
scons --target=mdk4
scons --target=mdk5
```

# ENV工具-menuconfig配置

menuconfig 是一种图形化配置工具，RT-Thread 使用其对整个系统进行配置、裁剪。

在env中执行menuconfig命令即可打开menuconfig配置工具.

![](figures/env_4.png)

执行menuconfig命令后的结果

![](figures/env_5.png)

选择好配置项之后按 ESC 键退出，选择保存修改即可自动生成 rtconfig.h 文件。此时再次使用 scons 命令就会根据新的 rtconfig.h 文件重新编译工程了。

# ENV工具-软件包管理配置

RT-Thread 提供一个软件包管理平台，这里存放了官方提供或开发者提供的软件包。该平台为开发者提供了众多可重用软件包的选择，这也是 RT-Thread 生态的重要组成部分。package工具作为Env的组成部分，为开发者提供了软件包的下载、更新、删除等管理功能。Env 命令行输入 `pkgs` 可以看到命令简介：

```shell
> pkgs
usage: env.py package [-h] [--update] [--list] [--wizard] [--upgrade]
                      [--printenv]

optional arguments:
  -h, --help  show this help message and exit
  --update    update packages, install or remove the packages as you set in
              menuconfig
  --list      list target packages
  --wizard    create a package with wizard
  --upgrade   update local packages list from git repo
  --printenv  print environmental variables to check
```

在下载、更新软件包前，需要先在 `menuconfig` 中 开启想要的软件包，这些软件包位于 `RT-Thread online packages` 菜单下，进入该菜单后，则可以看如下软件包分类：

![](figures/env_6.png)

找到你需要的软件包然后选中开启，保存并退出 menuconfig 。此时软件包已被标记选中，但是还没有下载到本地，所以还无法使用。

- **下载** ：如果软件包在本地已被选中，但是未下载，此时输入：`pkgs --update` ，该软件包自动下载；
- **更新** ：如果选中的软件包在服务器端有更新，并且版本号选择的是 latest 。此时输入： `pkgs --update` ，该软件包将会在本地进行更新；
- **删除** ：某个软件包如果无需使用，需要先在 menuconfig 中取消其的选中状态，然后再执行： `pkgs --update` 。此时本地已下载但未被选中的软件包将会被删除.

提示：pkgs --upgrade 命令和 pkgs --update 命令有什么区别

1. pkgs --upgrade 命令是用来升级 Env 功能脚本本身和软件包列表的。没有最新的包列表就不能选择最近更新的软件包。
2. pkgs --update 命令是用来更新软件包本身的，比如说你在 menuconfig 中选中了 json 和 mqtt 的软件包，但是退出 menuconfig 时并没有下载这些软件包。你需要使用 pkgs --update 命令，这时候 Env 就会下载你选中的软件包并且加入到你的工程中去。
3. 新版本的 Env 支持 menuconfig -s/--setting 命令，如果你不想每次更换软件包后使用 pkgs --update 命令，在使用 menuconfig -s/--setting 命令后配置 Env 选择每次使用 menuconfig 后自动更新软件包即可。

上述便是rt-thread env工具的详细的使用方法。

