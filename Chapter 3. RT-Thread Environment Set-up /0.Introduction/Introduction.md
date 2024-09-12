### Environment Setup Description

RT-Thread is a highly configurable and flexible real-time operating system, RT-Thread has a rich set of components compared to other RTOS, and RT-Thread has its own integrated IDE, which allows users to easily run the supported BSPs on their own hardware platforms. The following is a brief introduction to the RT-Thread development environment.

#### 1.Development aids and BSP support

RT-Thread provides powerful development aids for Board Support Package (BSP) development. These tools not only simplify the BSP development process, but also greatly improve the compatibility and stability of the BSP. Specifically, these tools include:

- Compile and build environment: RT-Thread supports a variety of compilers (e.g., GCC, Keil MDK, IAR, etc.) and provides a unified build system, which makes the project compilation and build process easy and fast.
- Graphical System Configuration: With RT-Thread Studio or env tools, users can graphically configure system components and parameters, such as thread stack size, peripheral drivers, network stacks, etc., which greatly reduces the complexity and error rate of configuration.
- Package Management: RT-Thread provides a rich ecosystem of software packages, covering everything from peripheral drivers to middleware to application layer software. Through the package management system, users can easily find, install, update and uninstall software packages, enabling rapid iteration and expansion of projects.

#### 2.RT-Studio Integrated Development Environment (IDE)

RT-Studio is the official integrated development environment of RT-Thread, which integrates code editing, compiling and building, debugging, graphical configuration and other functions into one, providing users with an efficient and convenient development platform. With RT-Studio, users can easily create, edit, build and debug RT-Thread projects without switching between multiple tools, which greatly improves development efficiency.

#### 3.Cross-platform development support

RT-Thread supports development on multiple operating systems and platforms, including MACOS, Linux and Windows. Users can choose the appropriate development environment according to their own preferences and habits. In particular, RT-Thread's env tool can generate configuration files for VSCode development environment, which enables users to take advantage of VSCode's powerful features and rich plug-ins for RT-Thread project development. At the same time, combined with the GNU tool chain and debugging tools such as OpenOCD, users can compile, build, download and debug code on different platforms to meet diverse development needs.

After completing this chapter, you will master the following:

- Developing RT-Thread using RT-Studio IDE

- Develop RT-Thread using VSCode + ENV.