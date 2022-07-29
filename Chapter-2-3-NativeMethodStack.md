#### JVM NativeMethodStack

##### 本地方法接口

- 概述：一个Native method 就是一个 Java 调用非 Java 代码的接口，比如 Unsafe 类 就有很多被 native 关键字修饰的 本地方法

- 为什么要用本地方法接口？

  - 与OS交互：

  JVM负责支持Java语言本身和JRE，但是有一些涉及系统调用的方法Java无法完成，需要借助C完成

  - 与环境外交互：

  与上一个原因相似，被native修饰的方法不一定必须使用系统调用，可以是任何需要的方法，native只是向JVM表明：我需要这个方法，在一些别的地方给出实现即可

  - Sun‘s Java：

  Sun的解释器就是C实现的，这使得它能像一些普通的C一样与外部交互。jre大部分都是用 Java 实现的，它也通过一些本地方法与外界交互。比如，类 `java.lang.Thread` 的 `setPriority()` 的方法是用Java 实现的，但它实现调用的是该类的本地方法 `setPriority()`，该方法是C实现的，并被植入 JVM 内部。

##### 本地方法栈

1. **线程私有**：本地方法栈是线程私有的栈
2. 类似于JVM Stack管理Java方法调用，JVM NativeMethodStack管理本地方法的调用
3. 类似于JVM Stack，JVM NativeMethodStack 同样支持固定/动态扩展内存大小
   1. 如果线程请求分配的栈容量超过本地方法栈允许的最大容量，Java 虚拟机将会抛出一个 `StackOverflowError` 异常
   2. 如果本地方法栈可以动态扩展，并且在尝试扩展的时候无法申请到足够的内存，或者在创建新的线程时没有足够的内存去创建对应的本地方法栈，那么 Java虚拟机将会抛出一个`OutofMemoryError`异常
4. native修饰方法 具体实现方式：
   1. 在Native Method Stack 中登记 native 方法，并在Execution Engine 执行时，加载本地方法库
   2. 当一个线程调用一个native方法时，其访问权限与C代码是一样的（甚至可以操作当前JVM中的内存，直接访问数据等等）