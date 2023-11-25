# 第七周IA-32/Linux中的地址转换

<a href="https://rosefinch-midsummer.github.io/book/file/cs/course2-week7.pdf" target="_blank">在线阅读PDF文档</a>

<!-- toc -->

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260023790.png)


## 第1讲  IA-32的地址转换和寻址方式

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260012096.png)


![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260013178.png)


## 第2讲  段选择符和段寄存器

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260013023.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260014423.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260014582.png)

## 第3讲  段描述符和段描述符表

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260015012.png)


![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260015281.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260016759.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260016205.png)

## 第4讲  逻辑地址向线性地址的转换

### 逻辑地址向线性地址转换（9m36s）

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260016145.png)


x8 

一个段描述符占64位（8个字节）。所以index要x8。

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260017499.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260018681.png)


![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260018748.png)

### 逻辑地址向线性地址转换举例（6m33s）

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260019826.png)

MMU：存储器管理部件，用于逻辑地址向线性地址转换

## 第5讲  线性地址向物理地址的转换

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260019801.png)


![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260019492.png)


![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260020523.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260020730.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260020452.png)


## 第6讲  Intel Core i7/Linux存储系统

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260021819.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260021059.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260021719.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260022368.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260022503.png)
![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202311260022564.png)

## 第七周小测验

错题序号：9


1对于IA-32中的指令“movl 8(%edx, %esi, 4), %edx”，若`R[edx]=0000 01B6H，R[esi]=0000 0008H`，其源操作数的有效地址EA是（ A ）。

A.0000 01DEH

B.0000 01B6H

C.0000 06E8H

D.0000 01F0H

解析：  A、源操作数的有效地址为`R[edx]+R[esi]*4+8=0000 01B6H+0000 0008H*4+8=0000 01DEH`。





2以下是有关IA-32段页式虚拟存储管理方式的叙述，其中错误的是（ C  ）。

A.指令中隐含给出的32位有效地址就是32位段内偏移量

B.逻辑地址由16位段选择符和32位段内偏移量组成

C.32位线性地址构成的地址空间就是4GB主存地址空间

D.进程的虚拟地址有48位逻辑地址和32位线性地址两种形式

解析：  C、32位线性地址空间是4GB的虚拟地址空间，也就是可执行目标程序（即进程）所在的虚拟地址空间，需要对其进一步进行分页存储管理，访问指令和数据时，应根据页面和页框之间的映射关系进行线性地址到物理地址的转换。





3以下是有关IA-32保护模式下地址转换过程的叙述，其中错误的是（B   ）。

A.顺序为逻辑地址→线性地址→物理地址

B.采用先分页、再分段的地址转换过程

C.32位物理地址就是指32位主存地址

D.地址转换前先计算出32位有效地址

解析：  B、地址转换顺序是先通过分段方式将48位逻辑地址转换为32位线性地址，然后再通过分页方式将32位线性地址转换为32位物理地址。




4以下有关IA-32段选择符的叙述中，错误的是（ D  ）。

A.CS寄存器中RPL字段表示当前特权级CPL

B.段选择符存放在一个16位段寄存器中

C.段选择符中的高13位为对应段表项的索引

D.程序的代码段和数据段共用同一个段选择符

解析：  D、IA-32中提供了多个段寄存器，如代码段寄存器CS、数据段寄存器DS、堆栈段寄存器SS等，它们可以分别存放代码段、数据段和堆栈段的段选择符。




5以下有关IA-32段描述符和段描述符表的叙述中，错误的是（ A  ）。

A.段基址低12位总是0，因此段描述符中的段基址字段占20位

B.段描述符表就是段表，段描述符就是其中的段表项

C.段描述符分普通段描述符和系统控制段描述符两类

D.段描述符表分GDT(全局)、LDT(局部)和IDT(中断)三类

解析：  A、段描述符中的段基址字段B31~B0占32位，而段限界字段L19~L0占20位。




6以下是有关IA-32中逻辑地址向线性地址转换的叙述，其中错误的是（  B ）。

A.系统启动时操作系统先对GDT和LDT进行初始化

B.每次逻辑地址向线性地址转换都要访问内存中的GDT或LDT

C.GTD和LDT在内存的起始地址分别存放在CPU内不同的地方

D.从对应段描述符中取出段基址与段内偏移量相加可得到线性地址

解析：  B、为了加快IA-32的地址转换过程，CPU中专门设置了段描述符cache，对应每个段寄存器（CS、DS、SS、ES、FS和GS）都有相应的描述符cache。每次段寄存器装入新的段选择符时，就会将对应的段描述符装入描述符cache中，在逻辑地址到线性地址转换时，MMU直接用描述符cache中的信息，不必访问内存中的GDT和LDT。





7以下是有关IA-32/Linux系统分段机制的叙述，其中错误的是（  C ）。

A.将内核代码段和内核数据段的段基址都设为0

B.段描述符中段存在位P为1，故不以段为单位分配内存

C.内核段描述符在GDT中，而用户段描述符在LDT中

D.将用户代码段和用户数据段的段基址都设为0

解析：  C、Linux系统中，将内核代码段、内核数据段、用户代码段和用户数据段对应的段描述符都包含在全局描述符表GDT中，分别位于GDT的第12、13、14和15项。






8已知变量y和数组a都是int型，a的首地址为0x8049b00。假设编译器将a的首地址分配在ECX中，数组的下标变量i分配在EDX中，y分配在EAX中，C语言赋值语句“`y=a[i];`”被编译为指令“`movl (%ecx, %edx, 4), %eax`”。在IA-32/Linux环境下执行该指令，则当i=150时，得到的存储器操作数的线性地址是（ A  ）。

A.0x8049d58

B.0x8049b9a

C.0x8049b00

D.0x804a100


解析：  A、Linux中将所有段的段基址都初始化为0，因而存储器操作数`a[150]`的线性地址=段基址+有效地址=`0+R[ecx]+R[edx]*4=0x8049b00+150*4=0x8049b00+600=0x8049b00+0x258=0x8049d58`。






9以下是有关IA-32中线性地址向物理地址转换过程的叙述，其中错误的是（ B  ）。

A.4GB线性地址空间被划分成1M个页面，每个页面大小为4KB

B.每次地址转换都需要先访问页目录表，然后访问页表，根据页表项得到物理地址

C.页目录表中的页目录项和页表中的页表项都占32位，且两者的结构完全相同

D.32位线性地址分成10位页目录索引、10位页表索引和12位页内偏移量三个字段

解析：  B、IA-32中有TLB，因此，每次线性地址向物理地址转换时，总是先访问TLB，当TLB命中时，就无需访问主存中的页目录表和页表。





10以下是有关IA-32存储管理控制寄存器的叙述，其中错误的是（C   ）。

A.CR3控制寄存器用于存放页目录表在主存的起始地址

B.若要启用分页机制，则CR0控制寄存器中的PE和PG都要置1

C.用户进程和操作系统内核都可以访问存储管理控制寄存器

D.CR2控制寄存器用于存放发生页故障（Page Fault）的线性地址

解析：  C、IA-32中提供的存储管理控制寄存器用于操作系统内核进行存储管理，访问这些控制寄存器的相关指令都是特权指令，只能在内核态使用，而用户进程中不能使用这些特权指令，因而用户进程不能访问这些控制寄存器。
















