# 第七周C语言语句的机器级表示

<a href="https://rosefinch-midsummer.github.io/book/file/cs/course1-week7.pdf" target="_blank">在线阅读PDF文档</a>

<!-- toc -->

## 第1讲 过程（函数）调用的机器级表示

### 1.过程调用概述（13分钟）

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310170826472.png)

测试程序：

```c
#include <stdio.h>
int add(int x,int y)
{
	return x + y;
}
int caller()
{
	int t1 = 125;
	int t2 = 80;
	int sum = add(t1,t2);
	return sum;
}
int main()
{
	int sum = caller();
	printf("%d",sum);
	return 0;
}
```

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310170829784.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310170835846.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310170840774.png)


![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310170842694.png)



### 2.过程（函数）的机器级代码结构（13分钟）

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310170905287.png)


![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310170906440.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310170918185.png)




### 3.过程调用的参数传递（12分钟）

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310170924406.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310170926560.png)



![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310170940829.png)

```c
#include <stdio.h>

void swap(int *x, int *y)
{
    int temp = *x;
    *x = *y;
    *y = temp;
}

int main()
{
    int a = 15, b = 22;
    printf("a=%d\tb=%d\n",a,b);
    swap(&a, &b);
    printf("a=%d\tb=%d\n",a,b);
    return 0;
}
```

```
a=15    b=22
a=22    b=15
```

按地址传递程序汇编结果：

```asm
	.file	"main.c"
	.text
	.globl	swap
	.type	swap, @function
swap:
.LFB0:
	.cfi_startproc
	endbr64
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	movq	%rdi, -24(%rbp)
	movq	%rsi, -32(%rbp)
	movq	-24(%rbp), %rax
	movl	(%rax), %eax
	movl	%eax, -4(%rbp)
	movq	-32(%rbp), %rax
	movl	(%rax), %edx
	movq	-24(%rbp), %rax
	movl	%edx, (%rax)
	movq	-32(%rbp), %rax
	movl	-4(%rbp), %edx
	movl	%edx, (%rax)
	nop
	popq	%rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE0:
	.size	swap, .-swap
	.section	.rodata
.LC0:
	.string	"a=%d\tb=%d\n"
	.text
	.globl	main
	.type	main, @function
main:
.LFB1:
	.cfi_startproc
	endbr64
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	subq	$16, %rsp
	movq	%fs:40, %rax
	movq	%rax, -8(%rbp)
	xorl	%eax, %eax
	movl	$15, -16(%rbp)
	movl	$22, -12(%rbp)
	movl	-12(%rbp), %edx
	movl	-16(%rbp), %eax
	movl	%eax, %esi
	leaq	.LC0(%rip), %rax
	movq	%rax, %rdi
	movl	$0, %eax
	call	printf@PLT
	leaq	-12(%rbp), %rdx
	leaq	-16(%rbp), %rax
	movq	%rdx, %rsi
	movq	%rax, %rdi
	call	swap
	movl	-12(%rbp), %edx
	movl	-16(%rbp), %eax
	movl	%eax, %esi
	leaq	.LC0(%rip), %rax
	movq	%rax, %rdi
	movl	$0, %eax
	call	printf@PLT
	movl	$0, %eax
	movq	-8(%rbp), %rdx
	subq	%fs:40, %rdx
	je	.L4
	call	__stack_chk_fail@PLT
.L4:
	leave
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE1:
	.size	main, .-main
	.ident	"GCC: (Ubuntu 11.3.0-1ubuntu1~22.04) 11.3.0"
	.section	.note.GNU-stack,"",@progbits
	.section	.note.gnu.property,"a"
	.align 8
	.long	1f - 0f
	.long	4f - 1f
	.long	5
0:
	.string	"GNU"
1:
	.align 8
	.long	0xc0000002
	.long	3f - 2f
2:
	.long	0x3
3:
	.align 8
4:
```


![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310170947511.png)

测试程序：

```c
#include <stdio.h>

void swap(int x, int y)
{
    int temp = x;
    x = y;
    y = temp;
}

int main()
{
    int a = 15, b = 22;
    printf("a=%d\tb=%d\n",a,b);
    swap(a, b);
    printf("a=%d\tb=%d\n",a,b);
    return 0;
}
```

按值传递程序汇编文件：

```asm
	.file	"main.c"
	.text
	.globl	swap
	.type	swap, @function
swap:
.LFB0:
	.cfi_startproc
	endbr64
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	movl	%edi, -20(%rbp)
	movl	%esi, -24(%rbp)
	movl	-20(%rbp), %eax
	movl	%eax, -4(%rbp)
	movl	-24(%rbp), %eax
	movl	%eax, -20(%rbp)
	movl	-4(%rbp), %eax
	movl	%eax, -24(%rbp)
	nop
	popq	%rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE0:
	.size	swap, .-swap
	.section	.rodata
.LC0:
	.string	"a=%d\tb=%d\n"
	.text
	.globl	main
	.type	main, @function
main:
.LFB1:
	.cfi_startproc
	endbr64
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	subq	$16, %rsp
	movl	$15, -8(%rbp)
	movl	$22, -4(%rbp)
	movl	-4(%rbp), %edx
	movl	-8(%rbp), %eax
	movl	%eax, %esi
	leaq	.LC0(%rip), %rax
	movq	%rax, %rdi
	movl	$0, %eax
	call	printf@PLT
	movl	-4(%rbp), %edx
	movl	-8(%rbp), %eax
	movl	%edx, %esi
	movl	%eax, %edi
	call	swap
	movl	-4(%rbp), %edx
	movl	-8(%rbp), %eax
	movl	%eax, %esi
	leaq	.LC0(%rip), %rax
	movq	%rax, %rdi
	movl	$0, %eax
	call	printf@PLT
	movl	$0, %eax
	leave
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE1:
	.size	main, .-main
	.ident	"GCC: (Ubuntu 11.3.0-1ubuntu1~22.04) 11.3.0"
	.section	.note.GNU-stack,"",@progbits
	.section	.note.gnu.property,"a"
	.align 8
	.long	1f - 0f
	.long	4f - 1f
	.long	5
0:
	.string	"GNU"
1:
	.align 8
	.long	0xc0000002
	.long	3f - 2f
2:
	.long	0x3
3:
	.align 8
4:
```


**注：这里a和b在栈中的位置有误，按后面程序中变量的分布都是上边的变量在下，下边的变量在上。根据别人实验结果和我生成的汇编文件，确实是a在下，b在上。而不是图中画的那样a在上b在下。**

**注：这里a和b在栈中的位置有误，按后面程序中变量的分布都是上边的变量在下，下边的变量在上。根据别人实验结果和我生成的汇编文件，确实是a在下，b在上。而不是图中画的那样a在上b在下。**

**注：这里a和b在栈中的位置有误，按后面程序中变量的分布都是上边的变量在下，下边的变量在上。根据别人实验结果和我生成的汇编文件，确实是a在下，b在上。而不是图中画的那样a在上b在下。**

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310181219358.png)

后面小测验内容也可作为参考。

5以下是一个C语言程序代码：

```c
int add(int x, int y)
{
    return x+y;
}
int caller( )
{
    int t1=100 ;
    int t2=200;
    int sum=add(t1, t2);
    return sum;
}
```

以下关于上述程序代码在IA-32上执行的叙述中，错误的是（  C  ）。

A.add函数返回时返回值存放在EAX寄存器中  
B.变量t1和t2被分配在caller函数的栈帧中  
C.传递参数时t1和t2的值从高地址到低地址依次存入栈中  
D.变量sum被分配在caller函数的栈帧中  

6第5题中的caller函数对应的机器级代码如下：

```asm
1      pushl       %ebp
2      movl       %esp, %ebp
3      subl         $24, %esp
4      movl        $100, -12(%ebp)
5      movl        $200, -8(%ebp) 
6      movl       -8(%ebp), %eax
7      movl        %eax, 4(%esp)
8      movl        -12(%ebp), %eax
9      movl        %eax, (%esp)  
10    call          add  
11    movl        %eax, -4(%ebp) 
12    movl        -4(%ebp), %eax
13    leave        
14    ret 
```

假定caller的调用过程为P，对于上述指令序列，以下叙述中错误的是（  B   ）。

A.从上述指令序列可看出，caller函数没有使用被调用者保存寄存器  
B.第3条指令将栈指针ESP向高地址方向移动，以生成当前栈帧  
C.第2条指令使BEP内容指向caller栈帧的底部  
D.第1条指令将过程P的EBP内容压入caller栈帧  

7对于第5题的caller函数以及第6题给出的对应机器级代码，以下叙述中错误的是（   D ）。

A.参数t1和t2的有效地址分别为`R[esp]和R[esp]+4`  
B.变量t1和t2的有效地址分别为`R[ebp]-12和R[ebp]-8`  
C.参数t1所在的地址低（或小）于参数t2所在的地址  
D.变量t1所在的地址高（或大）于变量t2所在的地址  
### 4.过程调用举例（11分钟）

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310171009797.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310171011839.png)


![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310171012064.png)

### 5.递归过程调用举例（11分钟）

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310171038080.png)


![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310171039149.png)

### 6.过程调用举例（14分钟）

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310171047930.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310171058219.png)


![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310171059788.png)

这里的数据按顺序压栈，和函数参数传递不同。

Code::Blocks運行結果如下所示：

```c
#include <stdio.h>

double fun(int i)
{
    volatile double d[1] = {3.14};
    volatile long int a[2];
    a[i] = 1073741824;
    return d[0];
}

int main()
{
    printf("%f\n",fun(1));
    printf("%f\n",fun(2));
    printf("%f\n",fun(3));
    printf("%f\n",fun(4));
    return 0;
}
```

```
3.140000
3.140000
3.140000
3.140000
```

```c
#include <stdio.h>

double fun(int i)
{
    volatile double d[1] = {3.14};
    volatile long int a[2];
    a[i] = 1073741824;
    printf("%d\n",&d[0]);
    printf("%d\n",&a[0]);
    printf("%d\n",&a[2]);
    return d[0];
}

int main()
{
    fun(3);
    return 0;
}
```

```
6487520
6487504
6487512
```

从结果来看，数据确实是按课上所讲的顺序压栈的，不过这里为何是一次+8呢？把`volatile long int`改为`int`，结果不变。

```c
#include <stdio.h>

double fun(int i)
{
    double d[1] = {3.14};
    int a[2];
    a[i] = 10;
    printf("%d\n",&d[0]);
    printf("%d\n",&a[0]);
    printf("%d\n",&a[2]);
    return d[0];
}

int main()
{
    fun(3);
    return 0;
}
```

```
6487520
6487504
6487512
```



这和栈帧那块内容很不同。



测试汇编代码的环境：Ubuntu 22.04.2 LTS (GNU/Linux 5.15.90.1-microsoft-standard-WSL2 x86_64)

汇编代码如下：

```asm
	.file	"main.c"
	.text
	.globl	fun
	.type	fun, @function
fun:
.LFB0:
	.cfi_startproc
	endbr64
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	subq	$64, %rsp
	movl	%edi, -52(%rbp)
	movq	%fs:40, %rax
	movq	%rax, -8(%rbp)
	xorl	%eax, %eax
	movsd	.LC0(%rip), %xmm0
	movsd	%xmm0, -40(%rbp)
	movl	-52(%rbp), %eax
	cltq
	movq	$1073741824, -32(%rbp,%rax,8)
	movsd	-40(%rbp), %xmm0
	movq	%xmm0, %rax
	movq	-8(%rbp), %rdx
	subq	%fs:40, %rdx
	je	.L3
	call	__stack_chk_fail@PLT
.L3:
	movq	%rax, %xmm0
	leave
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE0:
	.size	fun, .-fun
	.section	.rodata
.LC1:
	.string	"%f\n"
	.text
	.globl	main
	.type	main, @function
main:
.LFB1:
	.cfi_startproc
	endbr64
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	movl	$1, %edi
	call	fun
	movq	%xmm0, %rax
	movq	%rax, %xmm0
	leaq	.LC1(%rip), %rax
	movq	%rax, %rdi
	movl	$1, %eax
	call	printf@PLT
	movl	$2, %edi
	call	fun
	movq	%xmm0, %rax
	movq	%rax, %xmm0
	leaq	.LC1(%rip), %rax
	movq	%rax, %rdi
	movl	$1, %eax
	call	printf@PLT
	movl	$3, %edi
	call	fun
	movq	%xmm0, %rax
	movq	%rax, %xmm0
	leaq	.LC1(%rip), %rax
	movq	%rax, %rdi
	movl	$1, %eax
	call	printf@PLT
	movl	$4, %edi
	call	fun
	movq	%xmm0, %rax
	movq	%rax, %xmm0
	leaq	.LC1(%rip), %rax
	movq	%rax, %rdi
	movl	$1, %eax
	call	printf@PLT
	movl	$0, %eax
	popq	%rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE1:
	.size	main, .-main
	.section	.rodata
	.align 8
.LC0:
	.long	1374389535
	.long	1074339512
	.ident	"GCC: (Ubuntu 11.3.0-1ubuntu1~22.04) 11.3.0"
	.section	.note.GNU-stack,"",@progbits
	.section	.note.gnu.property,"a"
	.align 8
	.long	1f - 0f
	.long	4f - 1f
	.long	5
0:
	.string	"GNU"
1:
	.align 8
	.long	0xc0000002
	.long	3f - 2f
2:
	.long	0x3
3:
	.align 8
4:

```

注：我的写的程序的汇编代码和课程中的汇编代码存在很大差异，这里暂且存疑。等做完（四）中实验再加料。
## 第2讲 选择和循环语句的机器级表示

### 1.选择结构的机器级表示（18分钟）

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310171102804.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310171110075.png)


![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310171113815.png)


![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310180832772.png)

### 2.循环结构的机器级表示（14分钟）

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310180836675.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310180845205.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310180846085.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310180903193.png)

and（&）按位与

&&逻辑与
## 第七周小测验

1假设P为调用过程，Q为被调用过程，程序在IA-32处理器上执行，以下有关过程调用的叙述中，错误的是（ B  ）。

A.C语言程序中的函数调用就是过程调用  
B.从P传到Q的实参无需重新分配空间存放  
C.从Q跳回到Q执行应使用RET指令  
D.从P跳转到Q执行应使用CALL指令  

2以下是有关IA-32的过程调用方式的叙述，错误的是（  B ）。

A.入口参数使用栈（stack）传递，即所传递的实参被分配在栈中  
B.EBX、ESI、EDI、EBP和ESP都是被调用者保存寄存器  
C.返回地址是CALL指令下一条指令的地址，被保存在栈中  
D.EAX、ECX和EDX都是调用者保存寄存器  

3以下是有关IA-32的过程调用所使用的栈和栈帧的叙述，错误的是（ A  ）。

A.只能通过将栈指针ESP作为基址寄存器来访问用户栈中的数据  
B.从被调用过程返回调用过程之前，被调用过程会释放自己的栈帧  
C.过程嵌套调用深度越深，栈中栈帧个数越多，严重时会发生栈溢出  
D.每进行一次过程调用，用户栈从高地址向低地址增长出一个栈帧  

4以下是有关C语言程序的变量的作用域和生存期的叙述，错误的是（ D  ）。

A.不同过程中的非静态局部变量可以同名，是因为它们被分配在不同栈帧中  
B.非静态局部变量可以和全局变量同名，是因为它们被分配在不同存储区  
C.因为非静态局部变量被分配在栈中，所以其作用域仅在过程体内  
D.静态（static型）变量和非静态局部（auto型）变量都分配在对应栈帧中  

5以下是一个C语言程序代码：

```c
int add(int x, int y)
{
    return x+y;
}
int caller( )
{
    int t1=100 ;
    int t2=200;
    int sum=add(t1, t2);
    return sum;
}
```

以下关于上述程序代码在IA-32上执行的叙述中，错误的是（  C  ）。

A.add函数返回时返回值存放在EAX寄存器中  
B.变量t1和t2被分配在caller函数的栈帧中  
C.传递参数时t1和t2的值从高地址到低地址依次存入栈中  
D.变量sum被分配在caller函数的栈帧中  

6第5题中的caller函数对应的机器级代码如下：

```asm
1      pushl       %ebp
2      movl       %esp, %ebp
3      subl         $24, %esp
4      movl        $100, -12(%ebp)
5      movl        $200, -8(%ebp) 
6      movl       -8(%ebp), %eax
7      movl        %eax, 4(%esp)
8      movl        -12(%ebp), %eax
9      movl        %eax, (%esp)  
10    call          add  
11    movl        %eax, -4(%ebp) 
12    movl        -4(%ebp), %eax
13    leave        
14    ret 
```

假定caller的调用过程为P，对于上述指令序列，以下叙述中错误的是（  B   ）。

A.从上述指令序列可看出，caller函数没有使用被调用者保存寄存器  
B.第3条指令将栈指针ESP向高地址方向移动，以生成当前栈帧  
C.第2条指令使BEP内容指向caller栈帧的底部  
D.第1条指令将过程P的EBP内容压入caller栈帧  

7对于第5题的caller函数以及第6题给出的对应机器级代码，以下叙述中错误的是（   D ）。

A.参数t1和t2的有效地址分别为`R[esp]和R[esp]+4`  
B.变量t1和t2的有效地址分别为`R[ebp]-12和R[ebp]-8`  
C.参数t1所在的地址低（或小）于参数t2所在的地址  
D.变量t1所在的地址高（或大）于变量t2所在的地址  

8以下有关递归过程调用的叙述中，错误的是（   A  ）。

A.每次递归调用在栈帧中保存的返回地址都不相同  
B.可能需要执行递归过程很多次，因而时间开销大  
C.每次递归调用都会生成一个新的栈帧，因而空间开销大  
D.递归过程第一个参数的有效地址为`R[ebp]+8`  

9以下关于if (cond_expr) then_statement else else_statement选择结构对应的机器级代码表示的叙述中，错误的是（  D  ）。

A.计算cond_expr的代码段一定在条件转移指令之前  
B.一定包含一条条件转移指令（分支指令）  
C.一定包含一条无条件转移指令  
D.对应then_statement的代码一定在对应else_statement的代码之前  

10以下关于循环结构语句的机器级代码表示的叙述中，错误的是（  A ）。

A.循环体内执行的指令不包含条件转移指令  
B.一定至少包含一条条件转移指令  
C.循环结束条件通常用一条比较指令CMP来实现  
D.不一定包含无条件转移指令  









