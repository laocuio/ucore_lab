###exe1
	*操作系统镜像文件ucore.img是如何一步一步生成的？(需要比较详细地解释Makefile中每一条相关命令和命令参数的含义，以及说明命令导致的结果)
	  * 'cc kern/init/init.c'
		这是echo出的提示信息，"cc"是个符号链接，默认指向gcc，在Makefile文件中根据不同情况会被赋予不同的值

		'gcc -Ikern/init/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Ikern/debug/ -Ikern/driver/ -Ikern/trap/ -Ikern/mm/ -c kern/init/init.c -o obj/kern/init/init.o'
		"gcc"是编译器的名称 "-I"指定头文件的查找目录  "-fno-builtin" 不接受没有 __builtin_ 前缀的函数作为内建函数。 "-Wall"生成所有警告信息 "-ggdb"为调试器gdb 生成调试信息 "-m32"生成32位/64位机器上的代码 "-nostdinc"使编译器不再系统缺省的头文件目录里面找头文件,一般和-I联合使用,明确限定头文件的位置 "-fno-stack-protector"参数用来disable Stack-smashing protection  "-c"表明只编译不链接，即生成.o文件
		'''
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
		'''
		所有均同上

		'+ ld bin/kernel'
		同样是提示信息

		'ld -m    elf_i386 -nostdlib -T tools/kernel.ld -o bin/kernel  obj/kern/init/init.o obj/kern/libs/readline.o obj/kern/libs/stdio.o obj/kern/debug/kdebug.o obj/kern/debug/kmonitor.o obj/kern/debug/panic.o obj/kern/driver/clock.o obj/kern/driver/console.o obj/kern/driver/intr.o obj/kern/driver/picirq.o obj/kern/trap/trap.o obj/kern/trap/trapentry.o obj/kern/trap/vectors.o obj/kern/mm/pmm.o  obj/libs/printfmt.o obj/libs/string.o'
		"ld"连接命令 "-m" emulate the emulation linker, 与“elf_i386”表示模拟i386的环境  “-T”Use scriptfile as the linker script.  This script replaces ld's default linker script.这里表示将tools/kernel.ld作为描述文件
		这句话的整体含义是将obj/kern和obj/libs中的文件连接生成bin/kernel可执行文件
		'''
		+ cc boot/bootasm.S
		gcc -Iboot/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Os -nostdinc -c boot/bootasm.S -o obj/boot/bootasm.o 
		'''
		"-Os"表示对生成文件大小进行优化。它会打开 "-O2" 开的全部选项，除了会那些增加文件大小的。而"-O2" 则表示更多的优化，会尝试几乎全部的优化功能，但不会进行“空间换时间”的优化方法。 "-gstabs"此选项以stabs格式声称调试信息,但是不包括gdb调试信息 其他参数已经解释过
		'''
		+ cc boot/bootmain.c
		gcc -Iboot/ -fno-builtin -Wall -ggdb -m32 -gstabs -nostdinc  -fno-stack-protector -Ilibs/ -Os -nostdinc -c boot/bootmain.c -o obj/boot/bootmain.o
		'''
		同上
		'''
		+ cc tools/sign.c
		gcc -Itools/ -g -Wall -O2 -c tools/sign.c -o obj/sign/tools/sign.o
		'''
		"-g" 指示编译器，在编译的时候，产生调试信息。 其他参数同上
		'gcc -g -Wall -O2 obj/sign/tools/sign.o -o bin/sign'
		这句和上面一句将sign.c编译连接可执行文件sign
		'''
		+ ld bin/bootblock
		ld -m    elf_i386 -nostdlib -N -e start -Ttext 0x7C00 obj/boot/bootasm.o obj/boot/bootmain.o -o obj/bootblock.o
		'''
		-N：将text和data段设置为可读写权限，不对data数据段进行页对齐。
		-e start ：指定入口地址为start
		-Ttext 0x7C00 :使用0x7C00为text代码段的起始地址

		这之中有命令没有打出来，其作用是调用sign对bootblock.o进行处理，生成bootblock
		'''
		'obj/bootblock.out' size: 472 bytes
		build 512 bytes boot sector: 'bin/bootblock' success!
		'''
		以上两句是可执行文件sign对bootblock.o进行处理时的输出文件。

		'dd if=/dev/zero of=bin/ucore.img count=10000'
		使用dd工具生成一个5000KB的的空白镜像ucore.img,块大小默认为512B。
		dd if=bin/bootblock of=bin/ucore.img conv=notrunc
		将编译好的bootblock文件写入到ucore.img起始部分  
		conv=notrunc表示若bootblock大小大于ucore.img，不截断内容。
		dd if=bin/kernel of=bin/ucore.img seek=1 conv=notrunc
		将编译好的kernal文件写入到ucore.img起始部分的512字节后（即bootloader后）
		seek表示从输出文件开头跳过1个block（512字节）后开始复制。

		以上即时生成ucore.img的全部过程

Check out this neat program I wrote:

```
x = 0
x = 2 + 2
what is x
```

		