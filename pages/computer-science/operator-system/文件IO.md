- 文件I/O的基本接口
    - open/create/close/lseek/read/write
- 文件共享
    - 对于一个进程 ![image.jpg](../assets/9f0a071e-13fa-46ae-9625-5a6c2c90cf92-1115003.jpg)
        - 文件描述符表（进程空间）：fd->内核空间的v表项
        - 文件表项（内核空间）：-> v节点
            - 文件状态标志
            - 当前文件偏移量
            - 指向对应的文件的v-node
        - v-node
            - 包含一系列文件信息（包括i-node）
    - 多个单独进程打开一个文件（非父子进程，不用dup函数） ![image.jpg](../assets/AFFBE00A2AAB4CDA1608997061.jpg)
    - dup dup2
        - dup(fd)：复制文件描述符fd
        - dup(fd1, fd2)：复制fd1，fd2指定返回值
    - 脏数据同步到磁盘：sync/fsync/fdatasync
