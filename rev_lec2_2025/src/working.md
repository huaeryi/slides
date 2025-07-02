---
title: rev专题一 - 2025安全攻防实践
separator: <!--s-->
verticalSeparator: <!--v-->
theme: simple
highlightTheme: monokai-sublime
css:
    - custom.css
revealOptions:
    transition: 'slide'
    transitionSpeed: fast
    center: false
    slideNumber: "c/t"
    width: 1000

---

<!--s-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

<div class="middle center">
<div style="width: 100%">


# Reverse专题一 游戏/异架构逆向 

---

# @huayi  2025.7.9

</div>
</div>

<!--s-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

<div class="middle center">
<div style="width: 100%">


# Part.0 准备工作

</div>
</div>

<!--v-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

## 准备工作

一些需要安装的工具

- linux环境
- IDA
- GDB

<!--v-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

## 关于逆向

- 一杯茶、一包薯片、一个逆向做一天
- 赛题加密部分涉及密码学、数学知识；学无止境
- 大量可用工具；学无止境 plus

- 逆向核心逻辑十分枯燥

- 与开发联系紧密 
  - 语言 C/C++ Python Java C# Javascript go 各种汇编语言
  - 平台 Linux Windows Macos 
  - 架构 x86 ARM

<!--v-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

## 逆向参考资料

- 一杯茶、一包薯片、一个逆向做一天
- 赛题加密部分涉及密码学、数学知识；学无止境
- 大量可用工具；学无止境 plus

- 逆向核心逻辑十分枯燥

- 与开发联系紧密 
  - 语言 C/C++ Python Java C# Javascript go 各种汇编语言
  - 平台 Linux Windows Macos 
  - 架构 x86 ARM

<!--v-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

## 逆向课内容介绍

- 逆向基础 程序执行流程 工具使用 代码混淆技术等

- 逆向专题1 游戏/异架构逆向

- 逆向专题2 自动逆向

<!--v-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

## 逆向专题一内容

- 预处理、编译、汇编、链接
- 常见可执行文件结构elf pe   程序执行流程
- 静态分析工具IDA使用
- 动态分析 gdb x32dbg/x64dbg Ollydbg
- 两个简单逆向demo   （1 异或  2 base64变表）
- 约束求解z3-solver

- 有时间简单介绍
  - 代码混淆
  - 壳


<!--s-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

<div class="middle center">
<div style="width: 100%">


# Part.1 程序编译和执行流程

</div>
</div>

<!--v-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

## 程序编译

<img style="float: right; margin-right: 15px;" width="40%" src="rev-lec1/ida-view.webp">

一些会很好用的 subview：

- 查看字符串
- 程序流程图
- 交叉引用
- BinDiff

<br>

<div class="fragment">


- __libc_start_main 的内容其实不重要
  - 重要的是关注 _start
  - __libc_start_main 的参数
  - constructor / destructor

</div>

<!--v-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

## 程序执行

- 大量的指针 + 偏移，一般是结构体
- 一些特殊的函数调用
  - `__stack_chk_fail`
  - `__stack_chk_guard`
  - 表示程序有栈保护 / 函数中有数组
- 正确地修改函数参数和变量类型能有效提高代码可读性

<br>

- 对于有 strip 的程序，可以通过 IDA 的 FLIRT signature 还原部分函数名
  - 注意“避轻就重”
  - 通过特定字符串 / 特征标识函数
  - 敢于猜测复杂函数的功能，避免陷入细节

<!--v-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

## 俯瞰一下逆向的赛题 

- 字符串混淆
- 函数名混淆（strip）
- 代码混淆
  - 逻辑混淆
  - 控制流平坦化
  - 反调试 / 反虚拟机
- 加密算法隐藏 flag
  - 对称加密 / 非对称加密
  - 哈希算法

<!--s-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

<div class="middle center">
<div style="width: 100%">


# Part.2 动态调试技巧

</div>
</div>

<!--v-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

## GDB 的一些技巧

- gef 或 pwndbg 或 gdb-dashboard（推荐）
- starti / si / ni / s / n
- breakpoint 可以设置在函数名 / 地址
  - b *0x400000
  - i b 列出，d 删除
  - ena 启用 / dis 禁用
- 和 pwntools 的集成
  - gdb.debug(...)
  - gdbscript

<!--v-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

## .gdbinit

- .gdbinit 是 GDB 的初始化文件
  - 读取顺序：`~/.gdbinit` -> `./.gdbinit`
  - 可以在其中设置一些常用的命令
    - 如设置断点 / 执行到特定地址 / 设置寄存器值
- GDB 插件大多是通过 .gdbinit 加载的

<!--s-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

<div class="middle center">
<div style="width: 100%">


# Part.3 x86_64 以外的架构

</div>
</div>

<!--v-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

## 主流的架构

- ARM
  - armv7 / armv8 / aarch64 / armhf ...
  - STM32 / Raspberry Pi
- MIPS
  - mips32 / mips64 / mipsel ...
- PowerPC
- RISC-V

<!--v-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

## 异架构的调试

- 还是以静态调试为主，IDA / Ghidra 都支持多种架构的反汇编
- 需要了解目标架构的寄存器 / 指令集 / 运行环境
  - 使用 gdb-multiarch 调试
- 目标文件的格式不一定是 elf
  - PE / Mach-O / COFF
  - Intel HEX / Raw binary
  - 使用 file / readelf / objdump 查看
  - 使用 objcopy 转换格式

<!--s-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

<div class="middle center">
<div style="width: 100%">


# Part.4 其他语言的逆向

</div>
</div>

<!--v-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

## Java

- 编译型语言，编译得到 Java 字节码
  - 通过解释字节码实现跨平台
- 反编译工具
  - JD-GUI / Jadx / Fernflower
- Android 逆向
  - 采用的是 Dalvik 字节码 / Smali
  - apktool / frida / Android Studio
- 综合性较强的题目
  - 涉及到 Java 反射 / 字节码操作 / 本地方法

<!--v-->
<!-- .slide: data-background="figures/background.png" data-background-size="1140px"-->

## 其他语言

- Python 在 import 时会生成 .pyc 文件
  - 可以通过 uncompyle6 反编译
- 分析 AST / 字节码
  - ast / dis / marshal / opcode
- pyarmor：加密 Python 代码

<br>

- Golang / Rust / C++

<br>

- 小众语言的逆向
  - TPCTF 2023: Apple
  - APL 逆向

<!--s-->
<!-- .slide: data-background="rev-lec1/ending.webp" -->


<div class="middle center">
<div style="width: 100%">


# 谢谢大家

---

# questions?

</div>
</div>