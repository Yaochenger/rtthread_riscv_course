### VSCode+ENV Development RT-Thread Description

VSCode (Visual Studio Code) is a cross-platform free source code editor developed by Microsoft. Users can extend its functionality by installing extensions through the built-in extensions shop.

Env is an RT-Thread development tool that provides compilation and build environments, graphical system configuration, and package management for RT-Thread OS-based projects. Its built-in menuconfig provides an easy-to-use configuration trimming tool to freely trim kernels, components and packages, allowing the system to be built in a building block fashion.

The combination of VSCode and ENV can achieve an IDE-like feel. Compared to an IDE, VSCode+ENV is a more flexible development method, but this method is also more demanding on the user.

CH32V307 is a RISC-V architecture chip, at the same time the RT-Thread repository for the chip has a board support package (BSP), RT-Thread to the BSP to do a very rich support, the user can be modified on the basis of the creation of their own hardware platform in line with the BSP. the next step, we take the BSP as an example to build the compilation and debugging environment.

Before starting the next step, you need to install VSCode software by yourself.

### Source code acquisition

Before developing RT-Thread, you need to get the RT-Thread source code, you can directly clone the source code of [the main project](https://github.com/RT-Thread/rt-thread) or download the source code of [the specified release](https://github.com/RT-Thread/rt-). thread/tags). Some of the releases are listed below:

![](figures/vsc_2.png)

It is usually recommended to use the latest release, which fixes bugs and introduces new features from the previous release.

### BSP Configuration

First go to the BSP's applications sibling directory, then right click and select the ConEmu Here option to open the env tool.

![](figures/vsc_3.png)

Type code in env, press the arrow key right to complete, and finally press enter to enter the VSCode development environment.

![](figures/vsc_4.png)

After using this way to open the BSP, the user can use the scons command in the terminal to compile, before compiling the first need to set the path of the tool chain we use. The BSP's [toolchain](http://www.mounriver.com/download) can be obtained from the official MounRiver Studio software.

After preparing the toolchain, the current path where my toolchain is located is `D:\RT-ThreadStudio\repo\Extract\ToolChain_Support_Packages\WCH\RISC-V-GCC-WCH\8.2.0\bin `, and we use the following commands to set the path of the toolchain, user We use the following command to set the path of the toolchain, and the user can replace the path of the toolchain with his/her own, so that the path of the toolchain can be found when executing the scons compilation:

```shell
set RTT_EXEC_PATH=D:\RT-ThreadStudio\repo\Extract\ToolChain_Support_Packages\WCH\RISC-V-GCC-WCH\8.2.0\bin 
```

After setting the path to the toolchain is complete, execute scons compilation, the compilation result is as follows:

![](figures/vsc_5.png)

### VSCode Configuration

Enter the following command in the input box of the env tool opened above to generate the configuration file required for the VSCode project.

```shell
scons --target=vsc
```

After executing the above command, the .vsciode directory is generated in the project directory, and a json file exists in this directory, which is used to find the path to the code during debugging.

![](figures/vsc_6.png)

In addition to compiling and downloading, debugging is also an essential part of a perfect project, so we need to add debugging functionality to the project, which relies on a very popular debugging plugin for VSCode, `Cortex-Debug`, which is installed in VSCode's App Store, and which has good support for the RISC-V toolchain in its v1.4.4 version. V toolchain, so the v1.4.4 version of `Cortex-Debug` is recommended.

![](figures/vsc_7.png)

Click on the Debug option on the left and choose to create a lunch.json file for setting up the configuration required for debugging.

![](figures/vsc_8.png)

The default lunch.json file generated is as follows:

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Cortex Debug",
            "cwd": "${workspaceFolder}",
            "executable": "./bin/executable.elf",
            "request": "launch",
            "type": "cortex-debug",
            "runToEntryPoint": "main",
            "servertype": "jlink"
        }
    ]
}
```

Modify the generated lunch.json file.

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "RISCV Debug",
            "cwd": "${workspaceFolder}",
            "executable": "${workspaceFolder}\rtthread.elf",
            "request": "launch",
            "type": "cortex-debug",
            "runToEntryPoint": "main",
            "servertype": "openocd",
            "serverpath": "D:WCH/WCH-LINK_Debugger/1.0.4/OpenOCD/bin/openocd",
            "configFiles": [
                "D:WCH/WCH-LINK_Debugger/1.0.4/OpenOCD/bin/wch-riscv.cfg"
              ],
            "toolchainPrefix": "WCH/RISC-V-GCC-WCH/8.2.0/bin/riscv-none-embed"
        }
    ]
}
```

‘name": change the field to “RISCV Debug”.

‘executable": change the field to the path of the compiled rtthread.elf.

‘servertype": change the field to “openocd”.

‘serverpath": configure the field to be the path of the user's openocd.

‘configFiles": field to configure the openocd configuration files used by ch32.

‘toolchainPrefix": field configures the path to the GCC toolchain.

After completing the above configuration, press `F5` or `Fn+F5` to start debugging, the debugging interface is as follows:

![](figures/vsc_9.png)

After starting debugging, the programme will be downloaded to the chip as well.

The above procedure is also applicable to other risc-v bsp under RT-Thread repository, users can use VSCode+ENV to develop other BSPs by following the above steps according to their own needs.