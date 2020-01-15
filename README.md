# react-scheduler

这个是在React内部中使用的一个Scheduler库，它与React之间并没有强关联，也就是说它是一个普通的调度程序，提供一些通用的API，即使脱离React，也是可以使用的库；这个代码比较简单，从结构基本就能看出大概，方法也不多，可以结合test用例去学习，缺点就是没有文档，有写东西只能猜

## How to learn

1. 找到Hello World， 基本就是这个程序最基本或者最核心的方法; 因为没有文档，所以可以去用例找，在__tests__/Scheduler-test.js中第一个用例，加一些task，然后执行部分，就已经可以看出来这个库的最核心功能，以及核心的api；

2. 整理核心线索：找核心变量的意思，哪个变量不清楚了就全局搜索，看哪些地方用了，根据这些线索，理出这个变量的作用含义；

3. 走一遍核心流程

4. 整理出所有API，以及作用

## Scheduler

### 核心名词

* task： 普通任务
* timer:  定时任务
* host: 主程序
* scheduler: 调度程序
* currentTime: 当前时间，performance.now获取微妙级别的数据
* startTime: 定时任务希望开始的时间
* expirationTime：开始时间+timeout
* delay：定时程序要延后多久执行

### 核心方法1： unstable_scheduleCallback

功能描述： 根据option配置和callback生成一个task，放入schedule queue

流程：

![](https://img.alicdn.com/tfs/TB1mbr7tYj1gK0jSZFOXXc7GpXa-678-1092.jpg)

### 核心方法2： flushWork

![](https://img.alicdn.com/tfs/TB1YE64t7L0gK0jSZFxXXXWHVXa-684-624.jpg)

上面两个方法就展示了一个host放任务到schedule， 以及host在合适的时候，调用schedule的flushWork，让schedule开始执行；

## 打包后的Scheduler

打开npm/umd/scheduler.production.js或者development.js会发现代码都不见了， 只有一点点代码，这是什么情况？

我们发现里面的方法都调用的是

React['__SECRET_INTERNALS_DO_NOT_USE_OR_YOU_WILL_BE_FIRED'].Scheduler.xxxx();方法，那么这个方法在哪里？

可以回到React包里面的src/React.js， 发现里面有一个这个secret的定义，这个值是引用的ReactSharedInternals， 但是打开后发现并没有, 其实在forks/ReactSharedInternals，为什么这么做？怀疑是因为为了减小包的大小， React里面打包了这些Shared，ReactDOM也是，这样就重复了；具体怎么加进去的，还没研究；

## 总结

ReactFiber理解为协程的话，Scheduler是协程的一部分；Scheduler可以负责调度，但是调度什么，什么时候调度都是由ReactFiber在负责；




