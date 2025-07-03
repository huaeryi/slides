---
title: rev专题一 - 2025安全攻防实践
separator: <!--s-->
verticalSeparator: <!--v-->
theme: simple
highlightTheme: github
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

<div class="middle center">
<div style="width: 100%">


# Reverse专题一 游戏/异架构逆向 

---

# @huayi  2025.7.9

</div>
</div>

<!--s-->

<div class="middle center">
<div style="width: 100%">


# Part.0 准备工作

</div>
</div>

<!--v-->

## 准备工作

一些需要安装的工具

- windows环境
- CE
- x64dbg/ollydbg

<!--v-->

## 逆向专题一内容

- 异架构以及不同语言的逆向——本质 各种架构介绍
- 游戏逆向——目的  示例游戏的编写语言 游戏引擎
- 6502汇编 NES （BeginCTF红白机）

- Windows下的游戏逆向（pvz CE）

- Python逆向（反编译pyc）

- 古早Java游戏（诺基亚贪吃蛇）

- Javascript逆向（网页小游戏+代码混淆）

- Unity游戏逆向（C# dnspy）


<!--s-->

<div class="middle center">
<div style="width: 100%">


# Part.1 程序编译和执行流程

</div>
</div>

<!--v-->

## 程序编译


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

<div class="middle center">
<div style="width: 100%">


# Part.2 动态调试技巧

</div>
</div>

<!--v-->

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

## .gdbinit

- .gdbinit 是 GDB 的初始化文件
  - 读取顺序：`~/.gdbinit` -> `./.gdbinit`
  - 可以在其中设置一些常用的命令
    - 如设置断点 / 执行到特定地址 / 设置寄存器值
- GDB 插件大多是通过 .gdbinit 加载的

<!--s-->

<div class="middle center">
<div style="width: 100%">


# Part.3 x86_64 以外的架构

</div>
</div>

<!--v-->

## 主流的架构

- ARM
  - armv7 / armv8 / aarch64 / armhf ...
  - STM32 / Raspberry Pi
- MIPS
  - mips32 / mips64 / mipsel ...
- PowerPC
- RISC-V

<!--v-->

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

<div class="middle center">
<div style="width: 100%">


# Part.4 其他语言的逆向

</div>
</div>

<!--v-->

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