
####Lab8 report

###列出你认为本实验中重要的知识点，以及与对应的OS原理中的知识点

> 
>> 文件系统系统调用
>> 基于索引节点组织方式的Simple FS文件系统
>> 文件系统抽象层-VFS

##练习0：填写已有实验

> 使用了diff工具手动查找改进lab7和lab8。

##练习1: 完成读文件操作的实现（需要编码）

> 在sfs_io_nolock函数中，先计算一些辅助变量，并处理一些特殊情况（比如越界），然后有sfs_buf_op = sfs_rbuf,sfs_block_op = sfs_rblock，设置读取的函数操作。接着进行实际操作，先处理起始的没有对齐到块的部分，再以块为单位循环处理中间的部分，最后处理末尾剩余的部分。每部分中都调用sfs_bmap_load_nolock函数得到blkno对应的inode编号，并调用sfs_rbuf或sfs_rblock函数读取数据（中间部分调用sfs_rblock，起始和末尾部分调用sfs_rbuf），调整相关变量。完成后如果offset + alen > din->fileinfo.size（写文件时会出现这种情况，读文件时不会出现这种情况，alen为实际读写的长度），则调整文件大小为offset + alen并设置dirty变量。

##练习2: 完成基于文件系统的执行程序机制的实现（需要编码）

> 
>> 改写proc.c中的load_icode函数和其他相关函数，实现基于文件系统的执行程序机制。执行：make qemu。如果能看看到sh用户程序的执行界面，则基本成功了。如果在sh用户界面上可以执行”ls”,”hello”等其他放置在sfs文件系统中的其他执行程序，则可以认为本实验基本成功。
>> 使用load_icode_read()读取文件


