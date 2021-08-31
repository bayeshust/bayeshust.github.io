---
layout: post
title: "每天学习一个命令：jstack 打印 Java 进程堆栈信息"
tagline: ""
description: ""
category: 每天学习一个命令
tags: [jstack, java, debug, linux, thread-dump, ]
last_updated:
---

Jstack 用于打印出给定的 java 进程 ID 或 core file 或远程调试服务的 Java 堆栈信息。

这里需要注意的是 Java 8 引入了 Java Mission Control，Java Flight Recorder，和 `jcmd` 等工具来帮助诊断 JVM 和 Java 应用相关的问题。推荐使用最新的工具以及 `jcmd` 来进行诊断。

> Prints Java thread stack traces for a Java process, core file, or remote debug server. This command is experimental and unsupported.

jstack 命令能够：

- Troubleshoot with jstack Utility
- Force a Stack Dump
- Stack Trace from a Core Dump
- Mixed Stack

如果 java 程序崩溃生成 core 文件，jstack 工具可以用来获得 core 文件的 java stack 和 native stack 的信息，从而可以轻松地知道 java 程序是如何崩溃和在程序何处发生问题。另外，jstack 工具还可以附属到正在运行的 java 程序中，看到当时运行的 java 程序的 java stack 和 native stack 的信息，如果运行的 java 程序呈现 hung 的状态，jstack 是非常有用的。

thread dump 就是将当前时刻正在运行的 JVM 的线程拷贝一份，可以用来分析程序执行情况。

## 用法
打印某个进程的堆栈信息

    jstack [PID]

    jstack -l [PID]
    jstack -m [PID]
	jstack -F [PID]

关于如何找到 PID，有很多方法，使用 `jps -v` 或者 `ps -aux` 或者 `htop` 等等方法都可以。

说明：

- `-l` 选项会打印额外的信息，比如说锁信息， locks such as a list of owned java.util.concurrent ownable synchronizers，可以查看 `AbstractOwnableSynchronizer`
- `-F` Force a Stack Dump

## 分析 jstack 输出

在执行 `jstack -l [PID] > /tmp/output.txt` 之后可以对 `/tmp/output.txt` 进行分析

jstack 输出开头是当前 dump 的时间和 JVM 基本信息（包括版本等）:

    2018-05-24 14:41:06
    Full thread dump Java HotSpot(TM) 64-Bit Server VM (25.101-b13 mixed mode):

下面这个部分是 [Safe Memory Reclamation(SMR)](https://ieeexplore.ieee.org/document/1291819/) 安全的内存分配信息：

	Threads class SMR info:
	_java_thread_list=0x00007f3cd4005870, length=30, elements={
	0x00007f3d14011800, 0x00007f3d142dd800, 0x00007f3d142e1800, 0x00007f3d142f4000,
	0x00007f3d142f6000, 0x00007f3d142f8000, 0x00007f3d142fa000, 0x00007f3d14333800,
	0x00007f3d14340000, 0x00007f3d14bc6800, 0x00007f3c900a1000, 0x00007f3c90255000,
	0x00007f3c9025e800, 0x00007f3c90264000, 0x00007f3d14bdf800, 0x00007f3c64008800,
	0x00007f3c6400b000, 0x00007f3d14c1e800, 0x00007f3c54025800, 0x00007f3c54027000,
	0x00007f3c54042800, 0x00007f3c54044800, 0x00007f3c24005800, 0x00007f3c0c008800,
	0x00007f3c0c00a000, 0x00007f3c0c00b800, 0x00007f3c48027000, 0x00007f3c48010000,
	0x00007f3c48011000, 0x00007f3cd4004800
	}


接下来就是程序的**线程信息**（非 VM 线程，非 GC 线程）：

	"main" #1 prio=5 os_prio=0 cpu=1071286.79ms elapsed=509136.64s tid=0x00007f3d14011800 nid=0xad5 runnable  [0x00007f3d1993a000]
	   java.lang.Thread.State: RUNNABLE
			at org.eclipse.swt.internal.gtk.OS.Call(Native Method)
			at org.eclipse.swt.widgets.Display.sleep(Display.java:5570)
			at smartgit.Wx.d(SourceFile:305)
			at com.syntevo.smartgit.n.a(SourceFile:398)
			at com.syntevo.smartgit.n.a(SourceFile:247)

	"Reference Handler" #2 daemon prio=10 os_prio=0 cpu=94.43ms elapsed=509136.51s tid=0x00007f3d142dd800 nid=0xadc waiting on condition  [0x00007f3cf10f4000]
	   java.lang.Thread.State: RUNNABLE
			at java.lang.ref.Reference.waitForReferencePendingList(java.base@11.0.3/Native Method)
			at java.lang.ref.Reference.processPendingReferences(java.base@11.0.3/Reference.java:241)
			at java.lang.ref.Reference$ReferenceHandler.run(java.base@11.0.3/Reference.java:213)

	   Locked ownable synchronizers:
			- None


线程信息又可以划分成几个部分。

Section      | Example           | 解释
-------------|-------------------|---
线程名字     | main 和 Reference Handler | 可读的线程名字，这个名字可以通过 `Thread` 方法 `setName` 设定
线程 ID      | #1 | 每一个 `Thread` 对象的唯一 ID，这个 ID 是自动生成的，从 1 开始，通过 `getId` 方法获得
是否守护线程 | daemon      | 这个标签用来标记线程是否是守护线程，如果是会有标记，如果不是这没有
优先级       | prio=10     | Java 线程的优先级，可以通过 `setPriority` 方法设置
OS 线程的优先级 | os_prio |
CPU 时间     | cpu=94.43ms | 线程获得 CPU 的时间
elapsed      | elapsed=509136.51s |线程启动后经过的 wall clock time
Address      | tid |  Java 线程的地址，这个地址表示的是 JNI native Thread Object 的指针地址
OS 线程 ID   |  nid | The unique ID of the OS thread to which the Java Thread is mapped.
线程状态     | wating on condition | 线程当前状态 线程状态下面就是线程的堆栈信息
Locked Ownable Synchronizer | |

线程的运行状态：

- `New`: 线程对象创建，不可执行
- `Runnable`: 调用 thread.start() 进入 runnable，获得 CPU 时间即可执行
- `Running`: 执行
- `Waiting`: `thread.join()`或调用锁对象 `wait()` 进入该状态，当前线程会保持该状态直到其他线程发送通知到该对象
- `Timed_Waiting`：执行 Thread.sleep(long)、thread.join(long) 或 obj.wait(long) 等就会进该状态，与 Waiting 的区别在于 Timed_Waiting 的等待有时间限制；
- `Blocked`: 等待锁，进入同步方法，同步代码块，如果没有获取到锁会进入该状态。该线程尝试进入一个被其他线程占用的 synchronized 块，当前线程直到锁被释放之前一直都是 blocked 状态
- `Dead`：执行结束，或者抛出了未捕获的异常之后
- `Deadlock`: 死锁
- `Waiting on condition`：等待某个资源或条件发生来唤醒自己
- `Waiting on monitor entry`：在等待获取锁
- `terminated` 线程已经结束 `run()` 并且通知其他线程 joining


以上内容来自 [Oracle](https://docs.oracle.com/cd/E13150_01/jrockit_jvm/jrockit/geninfo/diagnos/using_threaddumps.html)

通过 jstack 信息可以分析线程死锁，或者系统瓶颈，但是这篇文章比较粗浅，只介绍了大概，等以后熟悉了补上。

## reference

- <https://docs.oracle.com/javase/8/docs/technotes/guides/troubleshoot/tooldescr016.html>
- <https://helpx.adobe.com/in/experience-manager/kb/TakeThreadDump.html>
