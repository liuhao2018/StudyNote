### RxJava

RxJava是什么：一个词可以概括为：异步

为什么要使用：简洁

概念：扩展的观察者模式，RxJava的异步实现，是通过一种扩展的观察者模式来实现的。

- Observable(被观察者)
- Observer(观察者)
- subscribe(订阅)

`Observable和Observer通过subscribute()方法实现订阅关系，从而Observable可以在需要的时候发出事件通知Observer。`

#### 事件回调

- onComplete():事件队列完结。
- onNext():事件发出。
- onError():事件队列异常

`在一个正确运行的事件序列中，onComplete()和onError()有且只有一个，并且是事件序列中最后一个。且两者之间只会调用一个。

#### 基本实现
1. 创建Observer 
2. 创建Observable 决定什么时候触发事件以及触发怎样的事件，使用create()方法来创建Observable。
	并定义事件触发规则。just(),from()可以快捷创建事件队列。
3. 实现订阅 observable.subscribe(observer) 



 
 
