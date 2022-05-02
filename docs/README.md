# TechMan-YuZe's Website

> An awesome project.

## 工具

### 开发工具

#### 1. JetBrains 全家桶

官网：https://jetbrains.com

中文官网：https://jetbrains.cn

```java
package com.yu.sharedModel;

import lombok.extern.slf4j.Slf4j;

import java.util.concurrent.locks.ReentrantLock;

@Slf4j(topic = "c.ReentrantLockTest")
public class ReentrantLockTest {
    public static ReentrantLock lock = new ReentrantLock();
    public static void main(String[] args) {
        ReentrantLockTest test = new ReentrantLockTest();
        lock.lock();
        try {
            log.debug("main");
            test.method1();
        } finally {
            lock.unlock();
        }
    }

    public void method1() {
        lock.lock();
        try {
            log.debug("method1");
            method2();
        } finally {
            lock.unlock();
        }
    }

    public void method2() {
        lock.lock();
        try {
            log.debug("method2");
            System.out.println("method2");
        } finally {
            lock.unlock();
        }
    }
}
```

#### 2. VS Code



#### 3. <a herf="/#/typora/">Typora</a>
<a>HTML Test</a>

[typora]: https://localhost:3000

## 学习

## 实战

