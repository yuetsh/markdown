# rxjs学习笔记

## 参考

[ReactiveX](http://reactivex.io/)

[RxJS](https://github.com/Reactive-Extensions/RxJS)

[中文文章](https://github.com/bboyfeiyu/android-tech-frontier/tree/master/androidweekly/%E9%82%A3%E4%BA%9B%E5%B9%B4%E6%88%91%E4%BB%AC%E9%94%99%E8%BF%87%E7%9A%84%E5%93%8D%E5%BA%94%E5%BC%8F%E7%BC%96%E7%A8%8B)

[英文原文](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754)

[文章1](http://nicholas.ren/2015/06/24/lets-talk-about-reactive.html)

## 响应式编程

相应式编程是一种面向数据流和变化传播的编程范式。

在命令式编程下，表达式a = b + c，a的值在这个表达式执行完毕之后就是确定的，即使b，c的值发生变化，a的值也不会改变。然而在响应式编程的语境下，a的值与b，c的值是绑定的，上述表达式其实建立的是a与b，c之间的一种依赖，a的值会随b和c的变化而变化。



## 观察者模式

现实生活中的订阅

* 报社的业务是出版报纸。

* 向某家报社订阅报纸，只要他们有新报纸出版，就会给你送来。只要你是他们的订户，你就会一直收到报纸。

* 当你不想再看报纸的时候，取消订阅，他们就不会再送报纸。

* 只要报社还在运营，就会一直有人（或者单位）向他们订阅报纸或取消订阅报纸。

所以：出版者＋订阅者＝观察者模式

观察者模式定义了对象之间的一对多依赖，这样依赖，当一个对象改变状态时，它的所有依赖者都会收到通知并自动更新。

一个有状态的主题对象（Subject） ===> 多个观察者（对象A，对象B，对象C...）

## RxJS
