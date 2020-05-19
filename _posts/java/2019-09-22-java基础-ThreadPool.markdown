---
layout: post
title: "java基础-ThreadPool"
date: 2019-09-22 22:00:00
categories: cn
tags: [java]
---

# java.util.concurrent线程池EXecutor    
* Executors线程工厂创建特定线程池    
    1. newFixedThreadPool()返回固定数量的线程池，线程数不变，任务提交线程池空闲立即执行，没有暂缓在一个任务队列等待
    2. newSingleThreadExecutor()一个线程的线程池空闲则执行,
    3. newCachedThreadPool()根据实际情况调整线程个数的线程池，不限制最大线程数，无任务不创建线程，60s回收
    4. newScheduledThreadPool()返回ScheduledExecutorService，可指定线程数量    

        ExecutorService executorService = Executors.newCachedThreadPool();   

        public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
        }

            public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue) {
        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
             Executors.defaultThreadFactory(), defaultHandler);
        }

        - corePoolSize 核心线程数(初始化线程数)    
        - maximumPoolSize 最大线程数 
        - keepAliveTime 空闲时间
        - TimeUnit 空闲单位 
        - workQueue 队列