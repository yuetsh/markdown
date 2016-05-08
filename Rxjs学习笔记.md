# rxjs学习笔记

## 参考

[ReactiveX](http://reactivex.io/)

[RxJS](https://github.com/Reactive-Extensions/RxJS)

[中文文章](https://github.com/bboyfeiyu/android-tech-frontier/tree/master/androidweekly/%E9%82%A3%E4%BA%9B%E5%B9%B4%E6%88%91%E4%BB%AC%E9%94%99%E8%BF%87%E7%9A%84%E5%93%8D%E5%BA%94%E5%BC%8F%E7%BC%96%E7%A8%8B)

[英文原文](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754)

[文章1](http://nicholas.ren/2015/06/24/lets-talk-about-reactive.html)

[关于Subject](https://segmentfault.com/a/1190000005069851)

## 观察者模式

现实生活中的订阅

* 报社的业务是出版报纸。

* 向某家报社订阅报纸，只要他们有新报纸出版，就会给你送来。只要你是他们的订户，你就会一直收到报纸。

* 当你不想再看报纸的时候，取消订阅，他们就不会再送报纸。

* 只要报社还在运营，就会一直有人（或者单位）向他们订阅报纸或取消订阅报纸。

所以：出版者＋订阅者＝观察者模式

观察者模式定义了对象之间的一对多依赖，这样依赖，当一个对象改变状态时，它的所有依赖者都会收到通知并自动更新。

一个有状态的主题对象（Subject） ===> 多个观察者（对象A，对象B，对象C...）

## 响应式编程

相应式编程是一种面向数据流和变化传播的编程范式。

在命令式编程下，表达式a = b + c，a的值在这个表达式执行完毕之后就是确定的，即使b，c的值发生变化，a的值也不会改变。然而在响应式编程的语境下，a的值与b，c的值是绑定的，上述表达式其实建立的是a与b，c之间的一种依赖，a的值会随b和c的变化而变化。

相应式编程是使用异步数据流进行编程，而任何事物都可以成为数据流，例如定义的变量、用户提交的表单、一个按钮的点击事件、缓存等等都是数据流，开发者可以观察数据流的变化并根据变化做出响应。例如，一个由微博订阅事件产生的数据流，开发者监听该数据流并在此基础上可以实现粉丝增加、订阅数统计、获取订阅内容等一系列的数据处理。

数据流（Stream）可以看作一个按时间排序的时间序列，一般的数据流模型包括三种事件类型：普通事件（值事件）、错误事件、完成事件。

* 观察者订阅该数据流之后，当普通事件发生时调用观察者处理普通事件的相关函数，如表单提交数据写入数据库等。

* 当数据流发生任何错误时会抛出异常，该异常由观察者捕获并在错误处理函数中处理，这类似于try/catch方法。

* 当整个数据流完成时，观察者会执行其数据流完成函数，如提供一些关闭窗口、断开数据库连接等功能。

* 有时候可以忽略错误事件和完成事件，而只需聚焦于如何定义和设计在发出值事件时要执行的函数。

## RxJS

Rx是一种编程的思维，而不是一个特定的框架或库。RxJS是Rx基于Javascript语言栈的实现。

### Observable

An observer subscribes to an Observable => 一个观察者订阅一个可观察对象。

而观察者（Observer）对可观察对象（Observable）发射的数据或数据序列作出响应。

### Subject

即是Observer（订阅多个Observable），也是Observable（转发Observer数据流，发射新数据）。

#### Subject VS BehaviorSubject

* Subject只会把在订阅发生的时间点之后来自原始Observable的数据发射给观察者。

 > 注意：Subject可能在一创建完成就立刻开始发射数据，因此这里有一个风险：在Subject被创建后到有观察者订阅它之前这个时间段内，有数据可能会丢失。

* 当观察者订阅BehaviorSubject时，它开始发射原始Observable最近发射的数据（如果此时还没有收到任何数据，它会发射一个默认值），然后继续发射其它任何来自原始Observable的数据。

* BehaviorSubject是Subject的一个衍生类，具有“最新的值”的概念。它总是保存最近向数据消费者发送的值，当一个Observer订阅后，它会即刻从BehaviorSubject收到“最新的值”。

* BehaviorSubject非常适于表示“随时间推移的值”。举一个形象的例子，Subject表示一个人的生日，而Behavior则表示一个人的岁数。（生日只是一天，一个人的岁数会保持到下一次生日之前。）

**例子：**

```javascript
var subject = new Rx.BehaviorSubject(0);

subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});

subject.next(1);
subject.next(2);

subject.subscribe({
  next: (v) => console.log('observerB: ' + v)
});

subject.next(3);
```
当Observer订阅它时，0是第一个被推送的值。紧接着，在第二个Observer订阅BehaviorSubject之前，它推送了2，虽然订阅在推送2之后，但是第二个Observer仍然能接受到2。所以结果如下：

```javascript
observerA: 0 // 第一次订阅，之前的初始值0
observerA: 1
observerA: 2
observerB: 2 // 第二次订阅，之前接收到的2
observerA: 3
observerB: 3
```