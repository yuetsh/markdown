# directives
## 作用在于语义化标签
其实也没什么好说的...道理都懂...
### 
## 例子
### 从上次的那个简单的例子说起
```javascript
.directive('vcBadgeMastery', ['Profile', function (Profile) {
        return {
            template: ' <span class="badge-mastery" ng-class="masteryColor" >{{masteryArray}}</span>',
            scope: {
                mastery: '='
            },
            restrict: 'E',
            link: function (scope, element, attrs) {
                scope.$watch('mastery', function (newVal) {
                    if (newVal) {
                        scope.masteryArray = Profile.mastery[newVal].label;
                        scope.masteryColor = Profile.mastery[newVal].color;
                    } else {
                        scope.masteryArray = [];
                    }
                });
            }
        };
    }])
```
**首先，directives直接return，或者是定义一个域；**

**比较好理解的directives的restrict属性；**

来上一张表：

| 字母        | 声明风格      |  示例                                  |                                        
| :--------:  | :--------:  | :------------------------------        |
| E           | 元素        |   < name title=PRODUCTS >< /name >     |
| A           | 属性        | < div name=PRODUCTS >< /div >          |
| C           | 样式        | < div class="name:PRODUCTS" > < /div > |
| M           | 注释        |  忽略...                               |
  
一清二楚，懂了

也就是上面的restrict:"E"限制他是一个Element，也就是vcBageMastery充当一个HTML的元素，用的时候就< vcBageMastery>< /vcBageMastery>就可以了

**来看一下scope这个对象；**
我找了一段网上的话：

scope - 如果设置为：

1. true - 将为这个directive创建一个新的scope。如果在同一个元素中有多个directive需要新的scope的话，它还是只会创建一个scope。新的作用域规则不适用于根模版（root of the template），因此根模版往往会获得一个新的scope。

2. {}(object hash) - 将创建一个新的、独立(isolate)的scope。”isolate” scope与一般的scope的区别在于它不是通过原型继承于父scope的。这对于创建可复用的组件是很有帮助的，可以有效防止读取或者修改父级scope的数据。这个独立的scope会创建一个拥有一组来源于父scope的本地scope属性(local scope properties)的object hash。这些local properties对于为模版创建值的别名很有帮助（useful for aliasing values for templates -_-!）。本地的定义是对其来源的一组本地scope property的hash映射(Locals definition is a hash of local scope property to its source)；
 
3. @或@attr - 建立一个local scope property到DOM属性的绑定。因为属性值总是String类型，所以这个值总是返回一个字符串。如果没有通过@attr指定属性名称，那么本地名称将与DOM属性的名称一直。例如 < widget my-attr=”hello {{name}}” >，widget的scope定义为：{localName:’@myAttr’}。那么，widget scope property的localName会映射出”hello {{name}}"转换后的真实值。name属性值改变后，widget scope的localName属性也会相应地改变（仅仅单向，与下面的”=”不同）。name属性是在父scope读取的（不是组件scope）

4. =或=expression（这里也许是attr） - 在本地scope属性与parent scope属性之间设置双向的绑定。如果没有指定attr名称，那么本地名称将与属性名称一致。例如< widget my-attr=”parentModel”>，widget定义的scope为：{localModel:’=myAttr’}，那么widget scope property “localName”将会映射父scope的“parentModel”。如果parentModel发生任何改变，localModel也会发生改变，反之亦然。（双向绑定）
 
5. &或&attr - 提供一个在父scope上下文中执行一个表达式的途径。如果没有指定attr的名称，那么local name将与属性名称一致。例如< widget my-attr=”count = count + value” >，widget的scope定义为：{localFn:’increment()’}，那么isolate scope property “localFn”会指向一个包裹着increment()表达式的function。一般来说，我们希望通过一个表达式，将数据从isolate scope传到parent scope中。这可以通过传送一个本地变量键值的映射到表达式的wrapper函数中来完成。例如，如果表达式是increment(amount)，那么我们可以通过localFn({amount:22})的方式调用localFn以指定amount的值（上面的例子真的没看懂，&跑哪去了？）。

呵呵，上面的话看不太懂

但是看代码有个
```javascript
scope：{
	mastery：'='
}
```
再看HTML里面

```html
<vc-badge-mastery mastery="item.mastery"></vc-badge-mastery>
```
感觉是个双向绑定，具体为什么要写成这个样子，不明白...


**link一个function**
这个好理解

```javascripe
link:function(scope,element,attrs){
	//function
}
```