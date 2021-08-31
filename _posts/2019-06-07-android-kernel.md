---
layout: post
title: "Android Kernel"
tagline: ""
description: ""
category: 学习笔记
tags: [android, kernel, linux-kernel, aosp, ]
last_updated:
---

说到 [Android Kernel](https://source.android.com/devices/architecture/kernel) 那就不得不说到 Linux Kernel，Android Kernel 基于 Linux Kernel 的[长期稳定版本](https://kernelnewbies.org/DevelopmentStatistics)，

## Linux Kernel
首先 Linux Kernel 是什么？ Linux Kernel 是在 GNU GPL v2 开源许可下开源的硬件底层驱动，包括了 CPU 调度，存储管理，IO 管理，等等。Linux Kernel 是 GPL 开源，所以为了适用移动设备内存，CPU 频率，耗电等特点，Google 将这部分 Linux Kernel 做了修改，并按照 GPL 将修改开源了。

> The kernel has complete control over the system.

Android 最早的内核是基于 Linux 2.6 内核的，在很长一段时间内，Android 的 Kernel 一直使用非常老版本的 Linux Kernel，但是随着时间发展，渐渐的每一个版本的 Android 发布都再使用最新的 Linux Kernel [^1].

[^1]: <https://en.wikipedia.org/wiki/Android_version_history>

## Android Kernel
回到 Android Kernel，不同设别出厂的时候就会带一个 stock 官方的 kernel，当然这个 Kernel 是稳定可以用于日常使用的。但是有些官方优化的 Kernel 并没有发挥硬件的最佳，所以 xda 上就有很多人发布不同的 Kernel，可以支持一些电池的优化，或者对硬件一些更好的支持。

### ElementalX
ElementalX 内核是一个我从 Nexus 6, OnePlus 3 开始就使用过的 Kernel，由 [flar2](https://forum.xda-developers.com/member.php?u=4684315) 开发。

ElementalX 内核的突出特点就是稳定，在不牺牲稳定性的前提下对系统做一些优化，比如滑动手势，亮度模式，震动模式，声音控制，文件系统格式等等。

个人使用的情况也是非常稳定，没有遇到过任何硬件不兼容问题。

- <https://elementalx.org/devices/>

### Franco Kernel
Franco Kernel 由 [franciscofranco](https://forum.xda-developers.com/member.php?u=3292224) 开发，是非常著名的一个 Kernel，支持非常多的设备。

- <https://kernels.franco-lnx.net/>

### blu_spark
blu_spark kernel 由 [eng.stk](https://forum.xda-developers.com/member.php?u=3873953) 开发。

更多的 kernel 可以查阅[这里](https://www.xda-developers.com/most-popular-custom-kernels-for-android/)

## CPU 调频器 {#cpu-governor}

### OnDemand

OnDemand 是一个比较老的 linux kernel 中的调频器，当负载达到 CPU 阈值时，调频器会迅速将 CPU 调整到最高频率。由于这种偏向高频的特性，使得它有出色的流动性，但与其他调频器相比可能对电池寿命产生负面影响。OnDemand 在过去通常被制造商选用，因为它经过了充分测试并且很可靠，但已经过时，并且正在被 Google 的 interactive 控制器取代。

### OndemandX
基本上是拥有 暂停、唤醒配置的 OnDemand，没有在 OnDemand 上做更多的优化。

### Performance
Performance 调频器将手机的 CPU 固定在最大频率。

### Powersave
与 Performance 调频器相反，Powersave 调频器将 CPU 频率锁定在用户设置的最低频率。

### Conservative
该调速器将手机偏置为尽可能频繁地选择尽可能低的时钟速率。换句话说，在 Conservative 调频器提高 CPU 时钟速度之前，必须在 CPU 上有更大且更持久的负载。根据开发人员实现此调频器的方式以及用户选择的最小时钟速度，Conservative 调频器可能会引入不稳定的性能。另一方面，它可以有利于电池寿命。

Conservative 调频器也经常被称为“slow OnDemand”。原始的、未经修改的 Conservative 是缓慢并且低效的。较新版本和修改版本 Conservative（来自某些内核）响应速度更快，并且几乎可以用于任何用途。

### Userspace
这种调频器在移动设备中极为罕见，它允许用户执行的任何程序设置 CPU 的工作频率。此调频器在服务器或台式 PC 中更常见，其中应用程序（如电源配置文件应用程序）需要特权来设置 CPU 时钟速度。

### Min Max
Min Max 调频器会根据负载选择最低或者最高的频率，而不会使用中间频率。

### Interactive
Interactive 会平衡内核开发人员（或用户）设置的时钟速度。换句话说，如果应用程序需要调整到最大时钟速度（CPU 100％负载），用户可以在调频器开始降低 CPU 频率之前执行另一个任务。由于此计时器，Interactive 还可以更好地利用介于最小和最大 CPU 频率之间的中间时钟速度。它的响应速度明显高于 OnDemand，因为它在调整到最大频率时速度更快。

Interactive 还假设用户打开屏幕之后很快就会与其设备上的某个应用程序进行交互。 因此，打开屏幕会触发最大时钟速度的斜坡，然后是上述的定时器行为。

Interactive 是当今智能手机和平板电脑制造商的首选默认调频器。

### InteractiveX
由内核开发人员“Imoseyon”创建，InteractiveX 调频器主要基于交互式调频器，增强了调整计时器参数，以更好地平衡电池与性能。但是，InteractiveX 调频器的定义功能是在屏幕关闭时将 CPU 频率锁定到用户最低定义的速度。

### Smartass
基于 Interactive，表现和之前的 minmax 一致，smartass 相应更快。电池寿命很难精确量化，但它确实在较低频率下可以使用更长。

当睡眠时调整到 352Mhz ，Smartass 还会限制最大频率（或者如果您设置的最小频率高于 352，它将限制到您设定的最小频率）。

该调频器会在屏幕关闭时缓慢的降低频率，甚至它也可以让手机 CPU 频率降至一个让手机无法正常使用的值（如果最小频率没有设置好的话）。

### SmartassV2
从 Erasmux 中而来的 Version 2 版本，该调频器的目标是“理想的频率”，并且更加积极地向这个频率增加，并且在此之后不那么激进。它在屏幕开启或者关闭时使用不同的频率，即 `awake_ideal_freq` 和 `sleep_ideal_freq`。 当屏幕关闭时，此调频器非常快地降低 CPU（快速达到 `sleep_ideal_freq` ）并在屏幕开启时快速向上调整到 `awake_ideal_freq`。 屏幕关闭时，频率没有上限（与 Smartass 不同）。因此，整个频率范围可供调频器在屏幕开启和屏幕关闭状态下使用。这个调频器的主打功能是性能和电池之间的平衡。

### Scary
Scary 基于 Conservative 并增加了一些 smartass 的特征，它相应地适用于 Conservative 的规则。所以它将从底部开始，采取一个负载样本，如果它高于上限阈值，一次只增加一个梯度，并一次减少一个。 它会自动将屏幕外的速度限制为内核开发人员设置的速度，并且仍然会根据保守法律进行调整。 所以它大部分时间都花在较低的频率上。 这样做的目的是通过良好的性能获得最佳的电池寿命。



### schedutil


Schedutil is the newest CPUFreq governor introduced back during Linux 4.7 as an alternative to ondemand, performance, and others. What makes Schedutil different and interesting is that it makes use of CPU scheduler utilization data for its decisions about CPU frequency control

## reference

- <https://android.googlesource.com/kernel/>
- <https://source.android.com/devices/architecture/kernel/releases.html>
- <https://www.xda-developers.com/most-popular-custom-kernels-for-android/>
- <https://forum.xda-developers.com/general/general/ref-to-date-guide-cpu-governors-o-t3048957>
