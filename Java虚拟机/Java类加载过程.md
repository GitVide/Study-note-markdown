# Java类加载过程

![Java类加载过程](../resources/Java%E7%B1%BB%E5%8A%A0%E8%BD%BD%E8%BF%87%E7%A8%8B.svg)

## 一、加载阶段

- 通过类的全限定名来获取二进制数据流
- 将class文件中的二进制数据读取到内存中
- 然后将该字节流所代表的静态存储结构转换为方法区中运行时的数据结构
- 在堆内存中生成一个该类的java.lang.class对象，作为访问方法区数据结构的入口。



![类被加载后在栈内存中的分布情况](../resources/%E7%B1%BB%E8%A2%AB%E5%8A%A0%E8%BD%BD%E5%90%8E%E5%9C%A8%E6%A0%88%E5%86%85%E5%AD%98%E4%B8%AD%E7%9A%84%E5%88%86%E5%B8%83%E6%83%85%E5%86%B5.svg)

**类加载的最终产物就是堆内存中的class对象，对同一个ClassLoader来说，不管某个类被加载了多少此，对应到堆内存中的class对象始终是同一个。**

**加载过程还没有结束，连接阶段便开始了，两者可以交叉工作，但总体来说，加载阶段肯定在连接阶段前面**