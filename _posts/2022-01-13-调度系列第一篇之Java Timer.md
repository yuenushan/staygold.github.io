---
layout: post
title: "调度系列第一篇之Java Timer"
date: 2022-01-13 
description: "调度原理之Java Timer"
tag: 调度
---   
## 前言
马上又是一年了，如果你要明早8点起床抢回家过年的火车票，你大概率会选择定一个闹钟来提醒你起床抢票。那如果是要让电脑每天晚上12点准时跑一段脚本，你会选择给自己定个闹钟，还是给电脑定个闹钟呢？

一个有经验的程序员应该已经想到了CronTab。如果要用Java代码来完成这个事情，你可以选择用Timer、ScheduledExecutorService等工具类。

那如果是让你自己写个调度工具，你会怎么实现呢？我们先想一想，后面再对比源码，看看有什么差异。

## 调度工具实现思路
调度工具常见的API方法：
```java
public interface IScheduler {

    /**
     * 简单理解：单次执行，指定时间延迟后执行一次
     * @param command 很好理解，定时要执行的方法
     * @param delay 也很好理解，相对当前时间需要等待initialDelay个时间单位后才开始执行
     * @param unit 时间单位，这个不需要解释
     */
    void schedule(Runnable command, long delay, TimeUnit unit);

    /**
     * 简单理解：每隔固定时间开始执行一次
     * @param command 很好理解，定时要执行的方法
     * @param initialDelay 也很好理解，第一次执行相对当前时间需要等待initialDelay个时间单位后才开始执行
     * @param period 两次连续执行的间隔时间
     * @param unit 时间单位，这个不需要解释
     */
    void scheduleAtFixedRate(Runnable command, long initialDelay, long period, TimeUnit unit);
    
    /**
     * 简单理解：一次执行结束需要休息固定时间后，才开始执行下一次
     * @param command 很好理解，定时要执行的方法
     * @param initialDelay 也很好理解，第一次执行相对当前时间需要等待initialDelay个时间单位后才开始执行
     * @param delay 等于 下次执行开始时间 减去 上次执行的结束时间
     * @param unit 时间单位，这个不需要解释
     */
    void scheduleWithFixedDelay(Runnable command, long initialDelay, long delay, TimeUnit unit);
}
```