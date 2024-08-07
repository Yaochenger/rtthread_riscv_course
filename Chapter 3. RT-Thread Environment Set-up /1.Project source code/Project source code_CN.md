# RT-Thread工程源码介绍

RT-Thread的工程代码在gitee与github上均存在，用户结合自己的使用环境选择具体的代码获取方式。[github工程代码获取方式](https://github.com/RT-Thread/rt-thread)与[gitee工程代码获取方式](https://gitee.com/rtthread/rt-thread)，用户在使用过程中遇到问题或者发现bug可以在上述平台提出issue,RT-Thread将及时去响应。

# 工程源码结构

工程源码结构如下：

![](figures/source_code.png)

bsp目录：该目录提供了针对不同MCU的RT-Thread Demo工程。

components目录：该目录提供了的独立于MCU的功能模块，如dfs, finsh等，以及rt-thread设备驱动，各种抽象层。

examples目录：该目录提供部分组件的使用示例与大部分的utest测试用例,且大部分demo都支持msh命令。

include目录：该目录包含rt-thread的头文件。

libcpu目录：该目录包含rt-thread支持的各种架构的移植文件，risc-v移植文件在该目录的risc-v子目录下。

src目录：该目录包含rt-thread的内核源码，rt-thread提供的各种基础服务来自于该文件夹。

tools：该目录包含各种python脚本，用于rt-thread系统自身开发使用，如源码编译、各种配置等。





