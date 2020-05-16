---
layout: post
title: JVM MXBean
tag: JVM
---

### MXBean

MBean是一种JavaBean,MBean往往代表的是JMX中的一种可以被管理的资源。MBean会通过接口定义，给出这些资源的一些特定操作：

属性的读和写操作

可以被执行的操作

关于自己的描述信息

在Java 5以后，JVM 中内置了一些特殊的 MBean，称为系统 MXBean (Platform MXBean)。这些系统 MXBean 实时地收集 JVM 实例内存使用，线程调度，类装载等各种信息。JMX 客户端程序可以通过这些系统 MXBean 获取系统运行的状况。

JConsole是一个JMX的客户端程序。通过JConole，可以直观的观测指定 JVM 的运行情况，同时也可以连接到用户自定义的MBean和MXBean，获取需要的监控信息。

JAVA 平台MXBean:
ClassLoadingMXBean 用于 Java 虚拟机的类加载系统的管理接口。
CompilationMXBean 用于 Java 虚拟机的编译系统的管理接口。
GarbageCollectorMXBean 用于 Java 虚拟机的垃圾回收的管理接口。
MemoryManagerMXBean 内存管理器的管理接口。
MemoryMXBean Java 虚拟机的内存系统的管理接口。
MemoryPoolMXBean 内存池的管理接口。
OperatingSystemMXBean 用于操作系统的管理接口，Java 虚拟机在此操作系统上运行。
RuntimeMXBean Java 虚拟机的运行时系统的管理接口。
ThreadMXBean Java 虚拟机线程系统的管理接口。

以下的例子,展示了如何使用java自身提供的MXBean来监控一个简单的java程序启动的过程中启动了哪些java线程

```
public class MXBeanDemo {
    public static void main(String[] args) {
        System.out.println("below is thread info:");
        //获取Java线程管理MXBean
        ThreadMXBean threadMXBean = ManagementFactory.getThreadMXBean();
        long[] threadIds = threadMXBean.getAllThreadIds();
        ThreadInfo[] threadInfos = threadMXBean.getThreadInfo(threadIds);
        //遍历线程信息，仅打印线程ID和线程名称信息
        for (ThreadInfo threadInfo : threadInfos) {
          System.out.println("["+threadInfo.getThreadId()+"]"+
          threadInfo.getThreadName());
        }
    }
}
```

输出信息：

[5] Attach Listener //添加事件
[4] Signal Dispatcher // 分发处理给 JVM 信号的线程
[3] Finalizer //调⽤对象 finalize ⽅法的线程
[2] Reference Handler //清除 reference 线程
[1] main //main 线程,程序⼊⼝

从上面的输出内容可以看出：一个Java程序的运行是main线程和多个其他线程同时运行。



