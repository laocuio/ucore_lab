###exe1
1. 操作系统镜像文件ucore.img是如何一步一步生成的？(需要比较详细地解释Makefile中每一条相关命令和命令参数的含义，以及说明命令导致的结果)
  * 答:	
	`cc kern/init/init.c`
	这是echo出的提示信息，"cc"是个符号链接，默认指向gcc，在Makefile文件中根据不同情况会被赋予不同的值。
	`gcc -Ikern/init/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/init/init.c -o obj/kern/init/init.o`
	"gcc"是编译器的名称 "-I"指定头文件的查找目录  "-fno-builtin" 不接受没有 __builtin_ 前缀的函数作为内建函数。 "-Wall"生成所有警告信息 "-ggdb"为调试器gdb 生成调试信息 "-m32"生成32位/64位机器上的代码 "-nostdinc"使编译器不再系统缺省的头文件目录里面找头文件,一般和-I联合使用,明确限定头文件的位置 "-fno-stack-protector"参数用来disable Stack-smashing protection  "-c"表明只编译不链接，即生成.o文件。
	```
	+ cc kern/libs/readline.c
	gcc -Ikern/libs/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/libs/readline.c -o obj/kern/libs/readline.o
	+ cc kern/libs/stdio.c
	gcc -Ikern/libs/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/libs/stdio.c -o obj/kern/libs/stdio.o
	+ cc kern/debug/kdebug.c
	gcc -Ikern/debug/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/debug/kdebug.c -o obj/kern/debug/kdebug.o
	+ cc kern/debug/kmonitor.c
	gcc -Ikern/debug/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/debug/kmonitor.c -o obj/kern/debug/kmonitor.o
	+ cc kern/debug/panic.c
	gcc -Ikern/debug/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/debug/panic.c -o obj/kern/debug/panic.o
	+ cc kern/driver/clock.c
	gcc -Ikern/driver/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/driver/clock.c -o obj/kern/driver/clock.o
	+ cc kern/driver/console.c
	gcc -Ikern/driver/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/driver/console.c -o obj/kern/driver/console.o
	+ cc kern/driver/intr.c
	gcc -Ikern/driver/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/driver/intr.c -o obj/kern/driver/intr.o
	+ cc kern/driver/picirq.c
	gcc -Ikern/driver/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/driver/picirq.c -o obj/kern/driver/picirq.o
	+ cc kern/trap/trap.c
	gcc -Ikern/trap/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/trap/trap.c -o obj/kern/trap/trap.o
	+ cc kern/trap/trapentry.S
	gcc -Ikern/trap/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/trap/trapentry.S -o obj/kern/trap/trapentry.o
	+ cc kern/trap/vectors.S
	gcc -Ikern/trap/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/trap/vectors.S -o obj/kern/trap/vectors.o
	+ cc kern/mm/pmm.c
	gcc -Ikern/mm/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/mm/pmm.c -o obj/kern/mm/pmm.o
	+ cc libs/printfmt.c
	gcc -Ilibs/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/  -c libs/printfmt.c -o obj/libs/printfmt.o
	+ cc libs/string.c
	gcc -Ilibs/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/  -c libs/string.c -o obj/libs/string.o
	所有均同上
	```
	`+ ld bin/kernel`
	同样是提示信息。
	`ld -m    elf_i386 -nostdlib -T tools/kernel.ld -o bin/kernel  obj/kern/init/init.o obj/kern/libs/readline.o obj/kern/libs/stdio.o obj/kern/debug/kdebug.o obj/kern/debug/kmonitor.o obj/kern/debug/panic.o obj/kern/driver/clock.o obj/kern/driver/console.o obj/kern/driver/intr.o obj/kern/driver/picirq.o obj/kern/trap/trap.o obj/kern/trap/trapentry.o obj/kern/trap/vectors.o obj/kern/mm/pmm.o  obj/libs/printfmt.o obj/libs/string.o`
	"ld"连接命令， "-m" emulate the emulation linker, 与“elf_i386”表示模拟i386的环境，“-T”Use scriptfile as the linker script.This script replaces ld's default linker script.这里表示将tools/kernel.ld作为描述文件。
	这句话的整体含义是将obj/kern和obj/libs中的文件连接生成bin/kernel可执行文件
	```
	+ cc boot/bootasm.S
	gcc -Iboot/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Os -nostdinc -c boot/bootasm.S -o obj/boot/bootasm.o 
	"-Os"表示对生成文件大小进行优化。它会打开 "-O2" 开的全部选项，除了会那些增加文件大小的。而"-O2" 则表示更多的优化，会尝试几乎全部的优化功能，但不会进行“空间换时间”的优化方法。 "-gstabs"此选项以stabs格式声称调试信息,但是不包括gdb调试信息 其他参数已经解释过
	```
	```
	+ cc boot/bootmain.c
	gcc -Iboot/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Os -nostdinc -c boot/bootmain.c -o obj/boot/bootmain.o
	同上
	```
	```
	+ cc tools/sign.c
	gcc -Itools/ -g -Wall -O2 -c tools/sign.c -o obj/sign/tools/sign.o
	"-g" 指示编译器，在编译的时候，产生调试信息。 其他参数同上
	```
	`gcc -g -Wall -O2 obj/sign/tools/sign.o -o bin/sign`
	这句和上面一句将sign.c编译连接可执行文件sign
	```
	+ ld bin/bootblock
	ld -m    elf_i386 -nostdlib -N -e start -Ttext 0x7C00 obj/boot/bootasm.o obj/boot/bootmain.o -o obj/bootblock.o
	-N：将text和data段设置为可读写权限，不对data数据段进行页对齐。
	-e start ：指定入口地址为star。
	-Ttext 0x7C00 :使用0x7C00为text代码段的起始地址
	```
	这之中有命令没有打出来，其作用是调用sign对bootblock.o进行处理，生成bootblock
	```
	'obj/bootblock.out' size: 472 bytes
	build 512 bytes boot sector: 'bin/bootblock' success!
	以上两句是可执行文件sign对bootblock.o进行处理时的输出信息。
	```
	`dd if=/dev/zero of=bin/ucore.img count=10000`
	使用dd工具生成一个5000KB的的空白镜像ucore.img,块大小默认为512B。
	`dd if=bin/bootblock of=bin/ucore.img conv=notrunc`
	将编译好的bootblock文件写入到ucore.img起始部分 。 
	conv=notrunc表示若bootblock大小大于ucore.img，不截断内容。
	`dd if=bin/kernel of=bin/ucore.img seek=1 conv=notrunc`
	将编译好的kernal文件写入到ucore.img起始部分的512字节后（即bootloader后）。
	seek表示从输出文件开头跳过1个block（512字节）后开始复制。
	
	以上即时生成ucore.img的全部过程

2. 一个被系统认为是符合规范的硬盘主引导扇区的特征是什么？
  * 答：
	```
	if (st.st_size > 510) {	
	fprintf(stderr, "%lld >> 510!!\n", (long long)st.st_size);
    return -1;
	}
	如果文件大小>510，则返回错误代码-1
	```

	```
	int size = fread(buf, 1, st.st_size, ifp); //文件大小<510，则返回错误代码-1
	if (size != st.st_size) {
		fprintf(stderr, "read '%s' error, size is %d.\n", argv[1], size);
		return -1;
	}
	文件大小<510，则返回错误代码-1
	```

	综合以上，可知block.0的文件大小必须为510字节

	```
	buf[510] = 0x55;
	buf[511] = 0xAA;
	末尾加了2个字节
	```

	综上所述，一个被系统认为符合规范的硬盘主引导扇区的特征是：
    1. 大小为510+2=512字节
    2. 末尾两个字节为0X55AA


###exe2
1. 从CPU加电后执行的第一条指令开始，单步跟踪BIOS的执行。
  * 答：加电后第一条指令位于0Xfffffff0，单步跟踪BIOS执行所得的代码如下：

    ```
	0xfffffff0:	ljmp   $0x3630,$0xf000e05b
	0xfe05b:	cmpw   $0xffc8,%cs:(%esi)
	0xfe060:	push   $0x0
	0xfe062:	jne    0xd241d2e3
	0xfe068:	mov    %edx,%ss
	0xfe06a:	mov    $0x7000,%sp
	0xfe06e:	add    %al,(%eax)
	0xfe070:	mov    $0x34c2,%dx
	0xfe074:	verw   %cx
	0xfe077:	jecxz  0xfe069
	0xfe079:	push   %bp
	```

2. 在初始化位置0x7c00设置实地址断点,测试断点正常。
  * 答：断点正常

3. 从0x7c00开始跟踪代码运行,将单步跟踪反汇编得到的代码与bootasm.S和 bootblock.asm进行比较
  * 答：
	反汇编得到的代码如下：

	```
	0x7c00:	cli    
	0x7c01:	cld    
	0x7c02:	xor    %ax,%ax
	0x7c04:	mov    %ax,%ds
	0x7c06:	mov    %ax,%es
	0x7c08:	mov    %ax,%ss
	0x7c0a:	in     $0x64,%al
	0x7c0c:	test   $0x2,%al
	0x7c0e:	jne    0x7c0a
	0x7c10:	mov    $0xd1,%al
	0x7c12:	out    %al,$0x64
	0x7c14:	in     $0x64,%al
	0x7c16:	test   $0x2,%al
	0x7c18:	jne    0x7c14
	0x7c1a:	mov    $0xdf,%al
	0x7c1c:	out    %al,$0x60
	0x7c1e:	lgdtw  0x7c6c
	0x7c23:	mov    %cr0,%eax
	0x7c26:	or     $0x1,%eax
	0x7c2a:	mov    %eax,%cr0
	0x7c2d:	ljmp   $0x8,$0x7c32
	0x7c32:	mov    $0xd88e0010,%eax
	0x7c36:	mov    %ax,%ds
	0x7c38:	mov    %ax,%es
	0x7c3a:	mov    %ax,%fs
	0x7c3c:	mov    %ax,%gs
	0x7c3e:	mov    %ax,%ss
	0x7c40:	mov    $0x0,%bp
	0x7c45:	mov    $0x7c00,%sp
	0x7c4a:	call   0x7ccd
	```
	最后调用bootmain.c。前的汇编代码与跟踪得到的反汇编代码前半部分基本相同（替换变量后），而bootblock.asm与跟踪得到的代码完全一样。这是因为bootloader由bootblock.o得到，bootblock.asm是由bootblock.o反汇编得到的，而bootblock.o由bootasm.o和bootmain.o链接得到，这两者由bootasm.S和bootmain.c编译得到。

4. 自己找一个bootloader或内核中的代码位置，设置断点并进行测试。
  * 答： 利用break设置端点，测试正常。


###exe3
1. 为何开启A20，以及如何开启A20
  * 答：在保护模式下，由于使用32位地址线，如果A20恒等于0，那么系统只能访问奇数兆的内存，即只能访问0--1M、2-3M、4-5M......，这显然是不行的，所以在保护模式下，这个开关必须打开。开启A20的汇编代码:
	```
	seta20.1:
	inb $0x64, %al                                  # Wait for not busy(8042 input buffer empty). 从0X64端口读取一个字节放到al寄存器中，按照协议标准是读取Status Register
    testb $0x2, %al                                #Status Register bit1 表示input register中有数据
    jnz seta20.1

    movb $0xd1, %al                                 # 0xd1 -> port 0x64
    outb %al, $0x64                                 # 0xd1 means: write data to 8042's P2 port 向0X64 register中写命令

seta20.2:
    inb $0x64, %al                                  # Wait for not busy(8042 input buffer empty).
    testb $0x2, %al
    jnz seta20.2

    movb $0xdf, %al                                 # 0xdf -> port 0x60
    outb %al, $0x60                                 # 0xdf = 11011111, means set P2's A20 bit(the 1 bit) to 1     A20Gate is enabled!
	```

	大致分为如下几步：
	  1. 等键盘控制器中input buffer清空
	  2. 向键盘控制器0X64端口输入0Xd1命令，表示向P2端口写数据
	  3. 等待input buffer清空
	  4. 向0X60端口写数据，其中代表A20的P2 bit1被置为1


2. 如何初始化GDT表
  * 答：初始化GDT表的汇编代码如下：
	```
	lgdt gdtdesc									#load  全局描述符寄存器
	
	.p2align 2                                          # force 4 byte alignment
	
	gdt:
	    SEG_NULLASM                                     # null seg
	    SEG_ASM(STA_X|STA_R, 0x0, 0xffffffff)           # code seg for bootloader and kernel
	    SEG_ASM(STA_W, 0x0, 0xffffffff)                 # data seg for bootloader and kernel
	
	gdtdesc:												#GDTR 高32位位全局描述符表的基地址，低16位为段界限 GDTR中的段界限以字节为单位。对于含有N个描述符的描述符表的段界限通常可设为8*N-1
	    .word 0x17                                      # sizeof(gdt) - 1  3个段描述符
	    .long gdt                                       # address gdt
	```
	大致分为如下步骤：
	  1. 新建段描述符，其中第一个段描述符位0，接着的两个分别位cs和ds段段描述符
	  2. 新建全局段描述符表，其中段界限位23，基地址为描述符的地址gdt
	  3. 将全局描述符表装载到全局描述符表寄存器
						
					
3. 如何使能和进入保护模式
  * 答：使能保护模式通过CR0寄存器的bit1来控制，将其置位1便使能了保护模式。经过前面的准备工作，将cs段选择子置为cs段在段描述符表中的偏移地址，并使用ljmp跳转，程序运行便进入了保护模式。



###exe4
1. bootloader如何读取硬盘扇区的？
  * 答：bootloader中有readsetc()函数表示读取一个扇区。读取一个扇区分为等待磁盘准备好、发出读取扇区的命令、等待磁盘准备好、把磁盘扇区数据读到指定内存四个步骤。其中最关键的读取数据由如下汇编代码完成。
	```
	static inline void
	insl(uint32_t port, void *addr, int cnt) {
	asm volatile (
        "cld;"
        "repne; insl;"
        : "=D" (addr), "=c" (cnt)
        : "d" (port), "0" (addr), "1" (cnt)
        : "memory", "cc");
	}
	```

	readseg()调用readsect()对硬盘完成访问。

2. bootloader是如何加载ELF格式的OS？
  * 答：现将整个文件内容读取出来，然后利用ELFHDR结构属性e_phoff、e_phnum找到proghdr，再利用proghdr的各种属性p_va、p_memsz、p_offset完成对最终的目标文件的加载。


###exe5
1. 要求完成函数kern/debug/kdebug.c::print_stackframe的实现，提交改进后源代码包（可以编译执行），并在实验报告中简要说明实现过程，并写出对上述问题的回答。
  * 答：主要是要理解函数堆栈。由`read_eip()`和`read_ebp`完成对当前eip和ebp寄存器的读取，然后利用ebp指向调用者的ebp，以及ebp和eip、args之间的关系一层层向上，最后得到每一层的相应值。最后一行代表进入内核前最后一层函数在调用下一层函数时的ebp和eip寄存器的值，以及相应的压栈参数，其中通过eip分析可知，他调用的应该是bootmian.c。


###exe6
1. 中断描述符表（也可简称为保护模式下的中断向量表）中一个表项占多少字节？其中哪几位代表中断处理代码的入口？
  * 答：一个表项占8个字节。第0-15和地47-63表示的偏移地址与第16-31位指示的段选择子共同指示了中断处理代码的入口。

2. 请编程完善kern/trap/trap.c中对中断向量表进行初始化的函数idt_init。在idt_init函数中，依次对所有中断入口进行初始化。使用mmu.h中的SETGATE宏，填充idt数组内容。每个中断的入口由tools/vectors.c生成，使用trap.c中声明的vectors数组即可。
  * 答：根据函数内的注释一步步来即可，主要是先使用vectors数组，利用SETGATE宏，传入vectors数组，对idt数组内容进行初始化。

3. 请编程完善trap.c中的中断处理函数trap，在对时钟中断进行处理的部分填写trap函数中处理时钟中断的部分，使操作系统每遇到100次时钟中断后，调用print_ticks子程序，向屏幕上打印一行文字”100 ticks”。
  * 答：计数到达100次调用`print_ticks()`即可。
	



		
