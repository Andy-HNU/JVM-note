#### JVM 路线
![image](https://i.bmp.ovh/imgs/2022/07/27/52e8c3349ad81bf8.png)
#### Jvm 总体结构
![image](https://i.bmp.ovh/imgs/2022/07/28/45cfb13a1c320273.jpg)
##### 类加载机制
- 类加载
    1. Math.class 读入内存
    2. 加载到JVM的方法区内（still in Memory）
    3. 在JVM堆区创建 Class.obj 对象

- 类加载过程
    1. 加载：IO读入字节码
    2. 链接：校验 准备 解析
    3. 初始化：运行静态代码 静态变量初始化
    4. 使用 和 关闭 run & close

- 类加载器结构
![image](https://i.bmp.ovh/imgs/2022/07/27/bff2416b10d94e16.png)
总体上讲， Java 的类加载器只分为两种:
第一种是 c++ 语言写的 启动类加载器：Bootstrap ClassLoader
第二种是 java 语言写的 其他各种类加载器。

第二种由可以分为 扩展类加载器， 应用程序类加载器 以及各种各样的自定义类加载器。

Bootstrap ClassLoader 启动类加载器 是用于加载 %JDK_HOME%\jre\lib 目录下的 rt.jar, tools .jar 等 jdk 核心常见类的。
Extension ClassLoader 扩展类加载器 是用于加载 %JDK_HOME%\jre\lib\ext 目录下的 sunec.jar 等 jdk 扩展类的。
ApplicationClassLoader 应用类加载器 是用于加载 用户自定义的类，比如 cn.how2j.how2tomcat.test.TestClassLoader 这个类，就是由 ApplicationClassLoader 加载的。

自定义类加载器 一般是需要进行动态加载或者其他业务目的类加载器。
