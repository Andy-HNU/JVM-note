### JVM程序计数器（PC）

#### 1.1作用

通过改变计数器的值 指向当前线程的下一条指令地址，该地址由执行引擎读取

![image](https://s3.bmp.ovh/imgs/2022/07/28/adfb734326276ee3.png)

（上图IDEA Jclasslib 分析）

#### 1.2 问题

- 为什么使用PC？
  - CPU 切换线程 （分时系统）
  - 每切换到一个线程，就需要恢复其寄存器&栈帧中内容，比如恢复PC中下一条指令地址，这样执行引擎才能知道下一条指令应该是哪一条
- 为什么**每个线程分配了一个私有的PC**？
  - 多线程实际上在一个时间片内 只有一个线程运行在core上
  - CPU会进行 中断 和 恢复
  - 每个线程在不同时间片执行的指令地址是不一样的，私有PC保证线程独立计算
- PC的生命周期？运行速度？占据空间？
  - 它是一块很小的内存空间，几乎可以忽略不计。也是运行速度最快的存储区域
  - 在 JVM 规范中，每个线程都有它自己的程序计数器，是线程私有的，生命周期与线程的生命周期一致
    - 程序控制流的指示器，分支、循环、跳转、异常处理、线程恢复等基础功能都需要依赖这个计数器来完成
    - 字节码解释器工作时就是通过改变这个计数器的值来选取下一条需要执行的字节码指令
    - 也就是说，PC存在的意义只为所属线程工作
  - **NO OutOfMemoryError**  **它是唯一一个在 JVM 规范中没有规定任何 `OutOfMemoryError` 情况的区域**
