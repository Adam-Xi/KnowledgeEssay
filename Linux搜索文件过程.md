1.文件中的数据是放在磁盘的数据区中的，而一个文件名则是通过对应的i节点与这些磁盘块联系起来，这些盘块的号码就存放在i节点的逻辑块数组i_zone[]中。在文件系统的一个目录中，其中所有文件名信息对应的目录项保存在该目录名文件的数据块中，例如，root/下的所有文件名的目录项就保存在root/目录名文件的数据块中，而文件系统根目录下的所有文件名信息则保存在指定i节点（1号节点)的数据块中，文件名的目录项结构如下:

```cpp
struct dir_entry
{
	unsigned short indoe;//i节点号
	char name[NAME_LEN];//文件名
};
```

目录项结构大小是16B，那么一个逻辑盘块可以存放1024/16=64个目录项。有关文件的其他信息则保存在该i节点号指定的i节点结构中，每个i节点号的i节点都位于磁盘的固定地方。

**在打开一个文件时，文件系统会根据给定的文件名找到其i节点号，从而通过其对应i节点信息找到文件所在的磁盘块位置。**

例如要查找文件/usr/bin/vi的i节点号，文件系统首先从固定i节点号(1号i节点)的根目录开始寻找，即从该i节点号1的数据块中找到名为usr的目录项，从而得到名为usr的目录项，从而得到文件/usr的i节点号，根据该i节点号文件系统可以顺利取得目录usr/的内容，并从其中找到bin的目录项，同理知道bin的i节点号，这样就知道了/usr/bin目录的位置，读取其i节点号的数据块内容，找到vi对应的目录项，从而得到vi的i节点号。

2.与文件相关的数据结构

内核使用文件结构file、文件表file_table[]和内存中的i节点表inode_table[]来管理对文件的访问操作。

```cpp
struct file
{
	unsigned short f_mode;
	unsigned short f_flags;
	unsigned short f_count;
	struct m_inode *f_inode;//对应的内存i节点
	off_t f_pos;
};
struct file file_table[NR_FILE];
```

在进程的task_struct中有个成员:struct file *filp[NR_OPEN]是进程使用的所有打开文件的文件结构指针表。文件描述符fd即该结构的索引值。下面一张图很好将三者关系展现出来:

![1584499320495](./image/1584499320495.png)