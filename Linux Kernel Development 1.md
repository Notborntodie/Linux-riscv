# Linux Kernel Development 1

## 从内核出发

### 内核源码树

<img src="http://cdn.zhengyanchen.cn/img202212302057369.png" alt="截屏2022-12-30 20.57.04" style="zoom:70%;" />



### 编译内核

```shell
make ARCH=riscv CROSS_COMPILE=riscv64-unknown-linux-gnu- defconfig
```

* 设置体系结构和交叉编译的默认配置

```shell
make ARCH=riscv CROSS_COMPILE=riscv64-unknown-linux-gnu- -j $(nproc)
```

* 编译，`-j`选项可以拆分成多个并行的编译任务

<img src="http://cdn.zhengyanchen.cn/img202212302133841.png" alt="截屏2022-12-30 21.33.11" style="zoom:30%;" />

### 内核开发特点

* 没有libc库

* GNU C

  1. 内联(`inline`)函数

     ```c
     static inline void wolf(unsigned int tail_size)
     ```

     * 函数很短且对时间要求高时使用

  2. 内联汇编

     c函数嵌入riscv汇编

     ```c
     unsigned long x;
     asm volatile("mv %0, ra" : "=r" (x) );
     ```

     * 如上是获取寄存器`ra`的值传递给x

  3. 分支声明

     ```c
     if (unlikely(error)){
       
     }
     if (likely(success)){
       
     }
     ```
  
     * 使用`likely`和`unlikely`,编译器将根据指令进行优化
       1. `unlikely`用于小概率发生的分支
       2. `likely`用于大概率发生的分支

* 没有内存保护机制
* 不要轻易使用浮点数
* 内核栈是固定的，8位8KB，64位16KB
* 同步和并发

* 可移植性
  * 大多数C代码与体系结构无关
  * 与体系结构相关的代码要移植出来
  * 64位对齐，不假定字节长和页面长等等有助于可移植性



## 进程管理





























