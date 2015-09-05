# 学习笔记 9月2日
## ListAdapter
一个ListView显示出来需要3个东西： 

1. listview（用来显示数据的列表）。 
2. Data(需要显示的数据)。 
3. 一个绑定Data和Listview的适配器ListAdapter。 

### ListView 

1. ListView的每一项其实都是TextView。 
2. 通过setAdapter方法来调用一个listAdapter来绑定数据。 

### ListAdapter 
ListAdapter继承于Adapter，它是ListView和其里面数据的适配器

1. ListAdapter是绑定Data和Listview的适配器。但是，它是接口，需要使用它的子类。 
常见的子类有：arrayAdapter，SimpleAdapter ，CursorAdapter 
2. android系统默认提供了很多默认的布局方式，也可以自定义。 

```
Android.R.layout.simple_list_item_1: 每一项只有一个TextView 
Android.R.layout.simple_list_item_2: 每一项有两个TextView 
Android.R.layout.simpte.list_item_single_choice: 每一项有一个TextView，但是这一项可以被选中。 
```

### ArrayAdapter 
ArrayAdapter是ListAdapter的一个直接子类，意思是数组适配器。

1. 数组适配器，它的作用就是一个数组和listview之间的桥梁，它可以将数组里边定义的数据一一对应的显示在Listview里边。 
2. ListView的每个TextView里边显示的内容就是数组里边的对象调用toString()方法后生成的字符串。 
3. 这是一个简单创建一个list的例子其中的代码： 

```java
ListView listview = new ListView(this); 

// 构造一个listview对象 
String[] data = {“google”,”amazon”,”facebook”}; 

// 构造一个数组对象，也就是数据 
listview.setAdapter(new ArrayAdapter(this, android.R.layout.simple_list_item_single_choice, data)); 

//构造一个array适配器，然后listview对象通过setAdapter方法调用适配器来和自己绑定数据 
list.setItemsCanFocus(true); 
list.setChoiceMode(listview.CHOICE_MODE_MULTIPLE); 
setContentView(listview); 
```

### SimpleAdapter 
SimpleAdapter也是ListAdapter的直接子类。通过SimpleAdapter可以让ListView当中的每一项里边的内容更加个性化。通常将ListView中某项的布局信息写在一个xml的布局文件当中。这个布局文件通过R.layout.file获得。

ArrayAdapter的作用是数组和ListView间的桥梁；而SimpleAdapter的作用是ArrayList和ListView间的桥梁。

1. 作用是ArrayList和ListView的桥梁。这个ArrayList里边的每一项都是一个Map类型。ArrayList当中的每一项Map对象都和ListView里边的每一项进行数据绑定一一对应。 
2. SimpleAdapter的构造函数： 
SimpleAdapter(Context  context, List  data, int resource, String[]  from, int[] to) 

参数：
 
1. context：上下文。 
2. data：基于Map的list。Data里边的每一项都和ListView里边的每一项对应。Data里边的每一项都是一个Map类型，这个Map类里边包含了ListView每一行需要的数据。 
3. resource ：就是一个布局layout，可引用系统提供的，也可以自定义。 
4. from:这是个名字数组,每个名字是为了在ArrayList数组的每一个item索引Map的Object用的。 
5. to：里面是一个TextView数组。这些TextView是以id的形式来表示的。例如：Android.R.id.text1,这个text1在layout当中是可以索引的。 

#### Adapter ListAdapter ArrayAdapter & SimpleAdapter
Adapter --(继承)--> ListAdapter

ListAdapter |--(直接子类)--> ArrayAdapter

ListAdapter |--(直接子类)-->  SimpleAdapter

具体理解：

什么是ListAdapter?

ListAdapter继承于Adapter，它是ListView和其里面数据的适配器。也就是要让一个ListView显示出来需要三个东西：

1. ListView (需要被显示的列表)。
2. Data, 和ListView绑定的数据，一般是一个Cursor或者一个字符串数组。
3. ListAdapter,是data和ListView的桥梁，起一个适配器的作用。

什么是ArrayAdapter?

ArrayAdapter是ListAdapter的一个直接子类，意思是数组适配器。

它的作用就是一个数组和ListView之间的桥梁。它将数组里定义的数据一一对应的显示在ListView里，通常有ArrayAdapter进行适配的ListView每一项通常只有一个TextView，而TextView里面显示的内容就是数组里面的对象调用toString()方法后生成的字符串。

什么是SimpleAdapter?

SimpleAdapter也是ListAdapter的直接子类。通过SimpleAdapter可以让ListView当中的每一项里边的内容更加个性化。通常将ListView中某项的布局信息写在一个xml的布局文件当中。这个布局文件通过R.layout.file获得。

ArrayAdapter的作用是数组和ListView间的桥梁；而SimpleAdapter的作用是ArrayList和ListView间的桥梁。

### pattern

![pattern](http://blog.yueyue.moe/usr/uploads/2015/09/3796246860.jpeg)

The ArrayAdapter fits in between an ArrayList (data source) and the ListView (visual representation) and configures two aspects:

1. Which array to use as the data source for the list
2. How to convert any given item in the array into a corresponding View object

参考链接：

1. [github](https://github.com/codepath/android_guides/wiki/Using-an-ArrayAdapter-with-ListView)
2. [ListView和ListAdapter](http://www.devdiv.com/listview_listadapter-blog-1-1323.html)

## Promise

### 什么是Promise
Promise是抽象异步处理对象以及对其进行各种操作的组件。

### 基于JavaScript的异步处理

JavaScript的执行环境是单线程（Single Thread），所谓单线程就是一次只能完成一件任务，如果有多个任务就必须要排队，先一个执行完，后一个再执行。这种模式很多时候就是傻缺，为了解决问题，JavaScript将任务的执行模式分成两种：同步（Synchronous）和异步（Asynchronous）。

异步模式在于每个任务有一个或者多个回调函数（Callback），前一个任务结束，不是执行下一个任务，而是执行回调函数，后一个任务是直接等前一个任务结束就执行，程序执行的顺序和任务排列的顺序是不一致的，所以就是异步的。

异步模式有好几种实现方式：

1. 回调函数（Callback）

	比如这个例子，下面三个函数：
	
	```javascript
	//TASK1
	f1()
	//TASK2
	f2()
	//TASK3
	F3()
	```
	这三个函数就是三个任务，然后假设f1先执行，f2需要依赖f1，f3与f1、f2无关。
	然后，就可以把f2作为f1的回调函数，f3的自然执行
	
	```javascript
	funcation f1(callback) {
		console.log("f1 - 我是吴彦祖");
		setTimeout(function(){
			console.log("f1 - 我比吴彦祖还帅");
			callback();
		},1000);
	}
	function f2(){
		console.log("f2 - 你们这群渣渣");
	}
	function f3() {
		console.log("f3 - 我有知识我自豪");
	}
	```
	这样执行下来的结果是
	
	```
	f1 - 我是吴彦祖
	f3 - 我有知识我自豪
	f1 - 我比吴彦祖还帅
	f2 - 你们这群渣渣
	```
	执行代码如下
	
	```
	f1(f2)
	f3
	```
	
2. 事件监听（EventListener）

	```javascript
	//jQuery写法
	$(document).on('finish', f2);
	
	funcation f1(callback){
		console.log("f1 - 我是吴彦祖");
		setTimeout(function(){
			console.log("f1 - 我比吴彦祖还帅");
			$(docunent).trigger('finish');
		},1000);
	}
	```
3. 发布/订阅（Observer Pattern）

	这个先不管...
4. Promise对象
	
	```javascript
	//jQuery写法
	function f1() {
　　　　var dfd = $.Deferred();
　　　　setTimeout(function () {
　　　　　　// f1的任务代码
　　　　　　dfd.resolve();
　　　　}, 1000);
　　　　return dfd.promise;
　　}
　　
　　//1.指定多个回调函数
　　f1().then(f2).then(f3);
　　
　　//2.指定发生错误时候的回调函数
　　f1().then(f2).fail(f3);
	```
	下面重点写(:з」∠)

### 理解Promise
为了更好地理解，摘抄一段：

>一天早晨，爹对儿子说：“宝儿，出去看看天气如何！”

>每个星期天的早晨，爹都叫小宝拿着超级望远镜去家附近最高的山头上看看天气走势如何，小宝说没问题，我们可以认为小宝在离开家的时候给了他爹一个promise。

>这时候，他爹就想了，如果明天艳阳高照，他就准备去钓鱼，如果天实在不行，就作罢，如果小宝对预报明天的天气也没底，他就在家宅一天哪也不去。

>大概过了半小时，小宝回来了。每周的结果不尽相同：

>A计划 ：天气晴朗

>小宝不辱使命，说外面阳光明媚，万里无云，这个promise was resolved（小宝信守承诺），爹就可以收拾行装，钓鱼去鸟。

>B计划： 小宝日观天象，阴转小雨的节奏

>小宝依然不辱使命，但是天公不作美，promise was resolved，但是孩儿他爹觉得还是搁家呆着吧。

>C计划：天象诡谲，小宝无法做出天气走势判断

>小宝败兴而归，云雾重重，遮蔽了视线，不敢妄言天气走势，小宝走的时候立下承诺说要给他爹预报天气，但是没有成功，我们说promise was rejected！孩儿他爹决定小心驶得万年船，还是在家吧。

>上述种种，用代码写出来是什么样子呢？

>我们可以把孩儿他爹看成controller，小宝就是service。

>整理逻辑：孩儿他爹让小宝去看天气，小宝不能立刻告诉他，但是孩儿他爹在等待结果的这段时间里可以抽抽烟，喝喝茶啥的，因为小宝承诺会把天气情况搞清楚。等小宝把天气预报带回来，他就可以决定下一步干啥。各位看官注意了：小宝登高望远看天气的时候并没有影响他爹干别的事情，这就是promise的妙处所在。

>Angular里有个then()函数，我们可以决定孩儿他爹到底是用哪个计划(A，B，C)，then()接收两个functions作为参数，第一个在promise is resolved的时候执行，另一个在promise is rejected的时候执行

>Controller: FatherCtrl 

>孩儿他爹掌控局面的代码如下：

>```javascript
>// function somewhere in father-controller.js
>       var makePromiseWithSon = function() {
>           
>// This service's function returns a promise, but we'll deal with that shortly
>			SonService.getWeather()
>               
>// then() called when son gets back
>				.then(function(data) {
>                   
>// promise fulfilled
>					if (data.forecast==='good') {
>							prepareFishingTrip();
>						} else {
>                		prepareSundayRoastDinner();
>						}
>         			}, function(error) {
>                   
>// promise rejected, could log the error with: console.log('error', error);
>                  prepareSundayRoastDinner();
>          		});
>       };
>```
>Service: SonService

>小宝的作用就是充当了一个service，他爬上山头看天象，有点类似调用weather API，而且还是异步调用，他得到的结果可能是个变量，也有可能出现异常情况(比如,返回500—>大雾弥漫)。

>从”Fishing Weather API”返回一个promise，如果it was resolved，就格式化成{“forecase”:”good”}。

>```javascript
>app.factory('SonService', function ($http, $q) {
>        return {
>            getWeather: function() {
>                
>// the $http API is based on the deferred/promise APIs exposed by the $q service
>                
>// so it returns a promise for us by default
>
>            return $http.get('http://fishing-weather-api.com/sunday/afternoon')
>            .then(function(response) {
>                if (typeof response.data === 'object') {
>                            return response.data;
>                        } else {
> 
>// invalid response
>                            return $q.reject(response.data);
>                        }
>                    }, function(response) {
> 
>// something went wrong
>                        return $q.reject(response.data);
>                    });
>            }
>        };
>    });
>```
>总结

>这个比喻向我们展示了异步的实质，孩儿他爹不会倚门等待儿子的归来，这段时间他完全可以自由活动。 孩儿他爹到底用哪个计划取决于(天气好/坏，没有成功预报)，小宝在临走的时候给他爹一个promise，就等他回来的时候决定是resolve还是reject。

>小宝其实是在使用异步服务（观天象—调用weather API）来获取天气信息，孩儿他爹根本就不懂技术，坐等结果即可！

### Promise的优点

Promise是一个接口，它用来处理的对象具有这样的特点：在未来某一时刻（主要是异步调用）会从服务端返回或者被填充属性。其核心是，promise是一个带有then()函数的对象。例子：

```javascript
var currentProfile = null;
var username = 'something';

fetchServerConfig(function(serverConfig) {
	fetchUserProfiles(serverConfig.USER_PROFILES, username,
		function(profiles) {
			currentProfile = profiles.currentProfile;
			});
	});
```
上面这种处理方式存在一些问题：

1. 对于代码缩进来说，这种代码就是一个噩梦，尤其在你需要链式调用很多次的时候；

2. 处于回调和函数之间的错误报告非常容易丢失，除非你在每一个步骤中都手动处理错误；

3. 如果需要使用currentProfile对象来做一些事情，那么你需要在最内层的回调中封装真正想要实现的逻辑，要么直接封装，要么通过一个单独的函数来封装。

Promise机制可以很好地解决这些问题。用promise来实现同样的事情：

```javascript
var currentProfile = 
	fetchServerConfig().then(function(serverConfig) {
		return fetchUserProfiles(serverConfig.USER_PROFILES, username);
	}).then(function(profiles) {
		return profiles.currentProfile;
	},function(error) {
//可以在这里处理错误，在fetchServerConfig或者fetchUserProfiles中处理都可以
	});
```

使用promise机制的优点如下：

1. 可以对函数进行链式调用，所以你不会陷入代码缩进噩梦中；

2. 在调用链的过程中，可以保证上一个函数调用完成之后才会调用下一个函数；

3. 每一个then()调用都带有两个参数（两个都是函数）。第一个是成功之后的回调，第二个是出错之后的处理器；

4. 如果调用链中出现了错误，错误将会被冒泡传递到其余的错误处理函数中。所以，最终来说，所有错误都可以在任意一个回调函数中进行处理。

### AngularJS中的Promise对象（$q）

Promise接口是AngularJS组织API的基础，从根本上讲，Promise接口从以下方面对异步请求做了规范：

1. 异步请求返回一个promise，而不是返回具体值；

2. Promise带有一个then函数，这个函数有两个参数：第一个参数是处理"resolved"和"sucess"事件的函数；第二个参数是处理"rejected"和"failure"事件的函数。调用这两个函数时将会把结果或者拒绝的原因作为参数传递进去；

3. 只要返回结果是合法的，接口就可以保证这两个函数中的一个会被调用。

大多数deferred/Q实现都会遵守以上方式，但是AngularJS的实现比较特殊，原因如下：

1. AngularJS知道$q的存在，所以$q会被整合到作用域模型中去。这样可以使解析时的传递速度更快，并且可以减少UI的闪烁和刷新；

2. AngularJS的模板也认识$q，这样一来，接口的内容就可以被当作最终结果值（而不是当作promise）来对待，然后等获取结果之后再通知promise；

3. 体积更小，因为对于常用的异步任务来说，AngularJS只实现了它们所需要的最基本、最重要的功能。

具体例子（官网doc上给出的例子）

```javascript
// for the purpose of this example let's assume that variables `$q` and `okToGreet`
// are available in the current lexical scope (they could have been injected or passed in).

function asyncGreet(name) {
  // perform some asynchronous operation, resolve or reject the promise when appropriate.
  return $q(function(resolve, reject) {
    setTimeout(function() {
      if (okToGreet(name)) {
        resolve('Hello, ' + name + '!');
      } else {
        reject('Greeting ' + name + ' is not allowed.');
      }
    }, 1000);
  });
}

var promise = asyncGreet('Robin Hood');
promise.then(function(greeting) {
  alert('Success: ' + greeting);
}, function(reason) {
  alert('Failed: ' + reason);
});
```

参考链接：

1. [Promise & $q](http://www.webdeveasy.com/javascript-promises-and-angularjs-q-service/) 
2. [Promise迷你书](http://liubin.github.io/promises-book/)