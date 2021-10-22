# Assembly_Language_Study
B站鱼C汇编语言视频自学笔记

# 1. 基础知识
汇编语言是直接在硬件之上工作的编程语言，首先要了解硬件系统的结构，才能有效的应用汇编语言对其编程。

## 1.1 机器语言
机器语言是机器指令的集合，是一台机器可以正确执行的命令。

## 1.2 汇编语言的产生
汇编语言的主体是汇编指令。<br>
汇编指令和机器指令的差别在于指令的表示方法上。<br>
汇编指令是机器指令的助记符<br>
寄存器：简单的讲是CPU中可以存储数据的器件，一个CPU中有多个寄存器。

## 1.3 汇编语言的组成
汇编语言由以下三类组成：
  - 汇编指令（机器码的助记符）
  - 伪指令（由编译器执行）
  - 其他符号（由编译器识别）加减乘除等

汇编语言的核心是汇编指令，它决定了汇编语言的特性。

## 1.4 存储器
CPU是计算机的核心部件，他控制整个计算机的运作并进行运算，要想让一个CPU工作，就必须向它提供指令和数据，指令和数据在存储器中存放，也就是平时所说的内存。<br>
磁盘不同于内存，磁盘上的数据或程序如果不读到内存中，就无法被CPU使用。

## 1.5 指令和数据
指令和数据是应用上的概念，在内存或磁盘上，指令和数据没有任何区别，都是二进制信息。也就是说，同样的数据作为指令和数据会表示不同的含义。（由底层电路控制）

## 1.6 存储单元
存储器被划分为若干个存储单元，每个存储单元从0开始顺序编号；

## 1.7 CPU对存储器的读写
CPU要想进行数据的读写，必须和外部期间进行三类信息的交互：
  - 存储单元的地址（地址信息）
  - 器件的选择，读或写命令（控制信息）
  - 读或写的数据（数据信息）

在计算机中专门有连接CPU和其他芯片的导线，通常称为总线
  - 物理上：一根根导线的集合；
  - 逻辑上划分为：
    - 地址总线（地址总线的宽度决定了CPU的寻址能力）
    - 数据总线（数据总线的宽度决定了CPU与其他期间进行数据传送时一次数据传送量）
    - 控制总线（控制总线宽度决定了CPU对系统中其他期间的控制能力）

## 1.8 地址总线
CPU是通过地址总线来指定存储单元的。<br>
地址总线上能传送多少个不同的信息，CPU就可以对多少个存储单元进行寻址。<br>
如果想要内存达到64位的速度，必须要有，缺一不可：
  - 64位的CPU
  - 64位的操作系统
  - 64位的软件

一个CPU有N根地址总线，则可以说这个CPU的地址总线的宽度为N。

## 1.9 数据总线
CPU与内存或其他器件之间的数据传送是通过数据总线来进行的。<br>
数据总线的宽度决定了CPU和外界的数据传送速度。

## 1.10 控制总线
CPU对外部期间的控制时通过控制总线来进行的。在这里控制总线是个总称，控制总线是一些不同控制线的集合。<br>
有多少根控制总线，就意味着CPU提供了对外部器件的多少种控制。<br>
所以，控制总线的宽度决定了CPU对外部器件的控制能力。

前面所讲的内存读或写命令是由几根控制线综合发出的：
  - 其中有一根名为读信号输出控制线负责由CPU向外传送读信号，CPU向该控制线上输出低电平表示将要读取数据；
  - 有一根名为写信号输出控制线负责由CPU向外传送写信号。

## 1.11 内存地址空间
一个CPU的地址线宽度为10，那么可以寻址1024个内存单元，这1024个可寻到的内存单元就构成这个CPU的内存地址空间。<br>
首先需要介绍两部分基本知识，主板和接口卡。

### 1.11.1 主板
每一台PC中，都有一个主板，主板上有核心器件和一些主要器件。<br>
这些器件通过总线相连。

### 1.11.2 接口卡
计算机系统中，所有可用程序控制其工作的设备，必须受到CPU的控制。<br>
CPU对外部设备不能直接控制，如显示器、音箱、打印机等。直接控制这些设备进行工作的是插在扩展插槽上的接口卡。

### 1.11.3 各类存储器芯片
从读写属性上看分为两类：
  - 随机存储器（RAM）
  - 只读存储器（ROM）

从功能和连接上分类：
  - 随机存储器RAM
  - 装有BIOS的ROM
  - 接口卡上的RAM

上述的那些存储器在物理上是独立的器件。<br>
但是他们在以下两点上相同：
  - 都和CPU的总线相连。
  - CPU对他们进行读或写的时候都通过控制线发出内存读写命令。

最终运行程序的是CPU，我们用汇编编程的时候，必须要从CPU角度考虑问题。<br>
对CPU来讲，系统中的所有存储器中的存储单元都处于一个统一的逻辑存储器中，它的容量受CPU寻址能力的限制。这个逻辑存储器即是我们所说的内存地址空间。

# 2. 寄存器（CPU工作原理）
一个典型的CPU由运算器、控制器、寄存器等器件组成，这些器件靠内部总线相连。<br>

区别：
  - 内部总线实现CPU内部各个器件之间的联系。
  - 外部总线实现CPU和主板上其他器件的联系。

8086 CPU有14个寄存器，他们的名称为：AX, BX, CX, DX, SI, DI, SP, BP, IP, CS, SS, DS, ES, PSW。

## 2.1 通用寄存器
8086CPU所有的寄存器都是16位的，可以存放两个字节。<br>
AX, BX, CX, DX通常用来存放一般性数据，所以被称为通用寄存器。<br>
8086上一代CPU中的寄存器都是8位的；<br>
为保证兼容性，这四个寄存器都可以分为两个独立的8位寄存器使用。
  - AX可以分为AH和AL；
  - BX可以分为BH和BL；
  - CX可以分为CH和CL；
  - DX可以分为DH和DL；

## 2.2 字在寄存器中的存储
一个字可以存在一个16位寄存器中，这个字的高位字节和低位字节自然就存在这个寄存器的高8位寄存器和低8位寄存器中。

## 2.3 几条汇编指令
```
汇编指令     控制CPU完成的操作                   用高级语言的语法描述
————————————————————————————————————————————————————————————————————————————————
mov ax,18   将18送入AX                          AX = 18
mov ah,78   将78送入AH                          AH = 78
add ax,8    将寄存器AX中的数值加上8              AX = AX + 8
mov ax,bx   将寄存器BX中的数据送入寄存器AX        AX = BX
add ax,bx   将AX，BX中的内容相加，结果存在AX中    AX = AX + BX
```
汇编指令不区分大小写<br>
数据溢出丢失，指的是进位值不能在8位寄存器中保存，但是CPU并不是真的丢弃这个进位值。

## 2.4 物理地址
CPU访问内存单元时要给出内存单元的地址。所有的内存单元构成的存储空间是一个一维的线性空间。<br>
我们将这个唯一的地址称为物理地址。

## 2.5 16位结构的CPU
概括的讲，16位结构描述了一个CPU具有以下几个方面特征：
1. 运算器一次最多可以处理16位的数据。
2. 寄存器的最大宽度为16位。
3. 寄存器和运算器之间的通路是16位的。

## 2.6 8086CPU给出物理地址的方法
8086有20位地址总线，可传送20位地址，寻址能力为1M。<br>
8086内部为16位结构，它只能传送16位的地址，表现出的寻址能力却只有64K。<br>

Q：那么8086CPU如何用内部16位的数据转换成20位的地址呢？

A：8086CPU采用一种在内部用两个16位地址合成的方法来形成一个20位的物理地址。
1. CPU中的相关部件提供两个16位的地址，一个称为段地址，另一个称为偏移地址；
2. 段地址和偏移地址通过内部总线送入一个称为地址加法器的部件；
3. 地址加法器将两个16位地址合并成一个20位的地址；
4. ...

### 2.6.1 地址加法器工作原理
地址加法器合成物理地址的方法：物理地址 = 段地址*16【即二进制数左移四位】 + 偏移地址

## 2.7 段的概念
内存没有分段，段的划分来自于CPU，由于8086CPU用“段地址*16 + 偏移地址”的方式给出内存单元的物理地址，使得我们可以用分段的方式来管理内存。

在编程时，可以根据需要，将若干地址连续的内存单元看作一个端，用段地址*16定位段的起始地址，用偏移地址定位段中的内存单元。

## 2.8 段寄存器
段寄存器就是提供段地址的。<br>
8086CPU有4个段寄存器：CS,DS,SS,ES。<br>
当8086CPU要访问内存时，由这4个段寄存器提供内存单元的段地址。

## 2.9 CS和IP
CS和IP是8086CPU中最关键的寄存器，它们指示了CPU当前要读取指令的地址。
  - CS为代码段寄存器
  - IP为指令指针寄存器

8086PC工作过程的简要描述
1. 从CS:IP指向内存单元读取指令，读取的指令进入指令缓冲器
2. IP = IP+所读取指令的长度，从而指向下一条指令
3. 执行指令，转到步骤1，重复这个过程
