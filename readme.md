# STM32F1 系列处理器模板工程

## 此工程的创建过程

1. 建立工程相关的文件夹

先创建一个 template 文件夹, 然后在 template 目录下创建 doc driver library listing output project startup user 这些目录.

这里可以直接使用 git bash 创建更快, 直接在 git bash 中敲这些命令即可, 复制它然后粘贴到 git bash 中执行

```
$ mkdir template
$ cd template
$ mkdir doc driver library listing output project startup user
```

|  文件夹  | 主要存放的文件                                         |
| :------- | :----------------------------------------------------- |
| doc      | 相关的说明文档, 相关知识的博客地址, 学习笔记等         |
| driver   | 驱动模块的驱动文件, 每一个模块分别占据一个子文件夹     |
| library  | ST 官方库文件, 仅仅需要将使用的库文件添加进来即可      |
| listing  | 链接时产生的一些中间文件, 不需要被 git 管理            |
| output   | 编译时产生的一些输出文件, 不需要被 git 管理            |
| project  | 工程配置文件, 保存工程的相关配置(比如说下载器的设置)   |
| startup  | ST 官方提供的库相关的核心代码, 包括引导启动的汇编代码  |
| user     | 上层应用程序代码相关文件, main 函数入口文件            |

2. 添加对应文件到指定的目录下

```
template
   |--- driver
   |       |--- systick                         /* 使用滴答定时器进行精确延时 */
   |       |        |--- bsp_systick.h          // 此驱动由 mz8023yt 提供
   |       |        |--- bsp_systick.c          // 此驱动由 mz8023yt 提供
   |       |--- usart                           /* 串口实现 printf 打印调试功能 */
   |       |        |--- bsp_usart.h            // 此驱动由 mz8023yt 提供
   |       |        |--- bsp_usart.c            // 此驱动由 mz8023yt 提供
   |--- library
   |       |--- inc                             /* st 库文件头文件 */
   |       |        |--- misc.h                 // stm32 中断核心文件
   |       |        |--- stm32f10x_gpio.h       // stm32 通用输入输出引脚库文件
   |       |        |--- stm32f10x_rcc.h        // stm32 系统时钟配置文件
   |       |        |--- stm32f10x_usart.h      // stm32 串口相关库文件
   |       |--- src                             /* st 库文件源文件 */
   |       |        |--- misc.c
   |       |        |--- stm32f10x_gpio.c
   |       |        |--- stm32f10x_rcc.c
   |       |        |--- stm32f10x_usart.c
   |--- startup                                 /* stm32 库核心代码 */
   |       |--- core_cm3.h
   |       |--- core_cm3.c
   |       |--- startup_stm32f10x_hd.s          // 汇编启动代码, 根据 STM32 flash 容量分为 ld md 和 hd
   |       |--- stm32f10x.h
   |       |--- system_stm32f10x.h
   |       |--- system_stm32f10x.c
   |--- user
           |--- stm32f10x_conf.h                // 工程配置头文件
           |--- stm32f10x_it.h                  // 中断控制头文件
           |--- stm32f10x_it.c                  // 中断控制源文件
           |--- stm32f10x_main.c
```

除了 mz8023yt 提供的代码以及 user/stm32f10x_main.c 文件, 其他的代码均可以在 ST 3.5 版本标准库中找到.

2. 使用 keil 新建工程，工程文件保存在刚刚创建的 project 目录下
3. 将文件加入工程中，注意 *.s 启动代码要必须要加入工程

使用 keil 打开新建立的工程, 点击"品"字型按钮, 配置工程内部文件组织结构, 以及添加对应的文件到工程中, 这里只需要添加 c 文件和 s 文件.

4. 设置链接和编译输出文件路径
5. 指定头文件查找路径
6. 设置默认开启的宏定义：STM32F10X_HD, USE_STDPERIPH_DRIVER
8. 修改 stm32f10x_conf.h 文件，只需要包含使用到的头文件
9. 新增 stm32f10x_main.c 文件，实现主函数
10. 编译工程，工程可以编译通过
11. 设置 DAP/J-LINK 下载配置，需要配置两个地方：Debug、Utilites
