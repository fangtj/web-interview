#javascript

1.前端安全,理解 xss 和 csrf

```
XSS：跨站脚本（Cross-site scripting，通常简称为XSS）是一种网站应用程序的安全漏洞攻击，是代码注入的一种。它允许恶意用户将代码注入到网页上，其他用户在观看网页时就会受到影响。这类攻击通常包含了HTML以及用户端脚本语言。
CSRF:跨站请求伪造（Cross-site request forgery），也被称为 one-click attack 或者 session riding，通常缩写为 CSRF 或者 XSRF， 是一种挟制用户在当前已登录的Web应用程序上执行非本意的操作的攻击方法。
说的直白一点儿
XSS： 通过客户端脚本语言（最常见如：JavaScript）在一个论坛发帖中发布一段恶意的JavaScript代码就是脚本注入，如果这个代码内容有请求外部服务器，那么就叫做XSS！
通常来说CSRF是由XSS实现的，所以CSRF时常也被称为XSRF [用XSS的方式实现伪造请求]（但实现的方式绝不止一种，还可以直接通过命令行模式（命令行敲命令来发起请求）直接伪造请求[只要通过合法验证即可]）。
XSS更偏向于代码实现（即写一段拥有跨站请求功能的JavaScript脚本注入到一条帖子里，然后有用户访问了这个帖子，这就算是中了XSS攻击了），CSRF更偏向于一个攻击结果，只要发起了冒牌请求那么就算是CSRF了。
```

2.数组快速排序

```
function quickSort(arr){
            //如果数组<=1,则直接返回
            if(arr.length<=1){return arr;}
            var pivotIndex=Math.floor(arr.length/2);
            //找基准，并把基准从原数组删除
            var pivot=arr.splice(pivotIndex,1)[0];
            //定义左右数组
            var left=[];
            var right=[];

            //比基准小的放在left，比基准大的放在right
            for(var i=0;i<arr.length;i++){
                if(arr[i]<=pivot){
                    left.push(arr[i]);
                }
                else{
                    right.push(arr[i]);
                }
            }
            //递归
            return quickSort(left).concat([pivot],quickSort(right));
        }
```

3.cookie,sessionStorage 和 localStorage 的区别

```
cookie在浏览器和服务器间来回传递。 sessionStorage和localStorage不会
sessionStorage和localStorage的存储空间更大；
sessionStorage和localStorage有更多丰富易用的接口；
sessionStorage和localStorage各自独立的存储空间；
sessionStorage：仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持；
localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据
```

4.javascript 中==和===的区别

```
一言以蔽之：==先转换类型再比较，===先判断类型，如果不是同一类型直接为false。
===表示恒等于，比较的两边要绝对的相同
alert(0 == ""); // true
alert(0 == false); // true
alert("" == false); // true

alert(0 === ""); // false
alert(0 === false); // false
alert("" === false); // false
先说 ===，这个比较简单，具体比较规则如下：
1、如果类型不同，就[不相等]
2、如果两个都是数值，并且是同一个值，那么[相等]；(！例外)的是，如果其中至少一个是NaN，那么[不相等]。（判断一个值是否是NaN，只能用isNaN()来判断）
3、如果两个都是字符串，每个位置的字符都一样，那么[相等]；否则[不相等]。
4、如果两个值都是true，或者都是false，那么[相等]。
5、如果两个值都引用同一个对象或函数，那么[相等]；否则[不相等]。
6、如果两个值都是null，或者都是undefined，那么[相等]。
再说 ==，具体比较规则如下：
1、如果两个值类型相同，进行 === 比较，比较规则同上
2、如果两个值类型不同，他们可能相等。根据下面规则进行类型转换再比较：
a、如果一个是null、一个是undefined，那么[相等]。
b、如果一个是字符串，一个是数值，把字符串转换成数值再进行比较。
c、如果任一值是 true，把它转换成 1 再比较；如果任一值是 false，把它转换成 0 再比较。
d、如果一个是对象，另一个是数值或字符串，把对象转换成基础类型的值再比较。对象转换成基础类型，利用它的toString或者valueOf方法。js核心内置类，会尝试valueOf先于toString；例外的是Date，Date利用的是toString转换。非js核心的对象，令说（比较麻烦，我也不大懂）
e、任何其他组合（array数组等），都[不相等]。
```

5.闭包

```
 闭包是指在 JavaScript 中，内部函数总是可以访问其所在的外部函数中声明的参数和变量，即使在其外部函数被返回了之后。然后闭包可以把一个局部变量传递到外部供其他函数或是变量使用,也可以把一个变量长时间的保留在系统的内存中
简单的说就是提升作用域
闭包的缺点：
1. 更多的内存消耗
2. 性能问题（跨作用域访问）
3. 滥用闭包函数会造成内存泄露，因为闭包中引用到的包裹函数中定义的变量都永远不会被释放，所以我们应该在必要的时候，及时释放这个闭包函数
闭包优点:
a. JavaScript允许你使用在当前函数以外定义的变量
b. 即使外部函数已经返回，当前函数仍然可以引用在外部函数所定义的变量
c. 闭包可以更新外部变量的值
d. 用闭包模拟私有方法
```

6.哪些操作会导致内存泄漏

```
1. setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏。
2. 闭包
3. 控制台日志
4. 循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）
```

7.面向对象过程的三大基本特征

```
继承,多态,封装
```

8.如何判断一个变量是对象还是数组

```
prototype.toString.call(),这个方法兼容性最好,千万不要使用typeof,都会返回object
```

9.ES5 和 ES6 中继承有啥区别

```
ES5的继承时通过prototype或构造函数机制来实现。ES5的继承实质上是先创建子类的实例对象，然后再将父类的方法添加到this上（Parent.apply(this)）。
ES6的继承机制完全不同，实质上是先创建父类的实例对象this（所以必须先调用父类的super()方法），然后再用子类的构造函数修改this。
具体的：ES6通过class关键字定义类，里面有构造方法，类之间通过extends关键字实现继承。子类必须在constructor方法中调用super方法，否则新建实例报错。因为子类没有自己的this对象，而是继承了父类的this对象，然后对其进行加工。如果不调用super方法，子类得不到this对象
```

10.垂直居中有哪些方法

```
单行文本的话可以使用height和line-height设置同一高度。
position+margin：设置父元素:position: relative;，子元素height: 100px; position:absolute;top: 50%; margin: -50px 0 0 0;（定高）
position+transform：设置父元素position:relative,子元素：position: absolute;top: 50%;transform: translate(0, -50%);（不定高）
百搭flex布局(ie10+)，设置父元素display:flex;align-items: center;（不定高）
```

11.值类型和引用类型的区别

```
（1）值类型：
    1、占用空间固定，保存在栈中（当一个方法执行时，每个方法都会建立自己的内存栈，在这个方法内定义的变量将会逐个放入这块栈内存里，随着方法的执行结束，这个方法的内存栈也将自然销毁了。因此，所有在方法中定义的变量都是放在栈内存中的；栈中存储的是基础变量以及一些对象的引用变量，基础变量的值是存储在栈中，而引用变量存储在栈中的是指向堆中的数组或者对象的地址，这就是为何修改引用类型总会影响到其他指向这个地址的引用变量。）
    2、保存与复制的是值本身
    3、使用typeof检测数据的类型
    4、基本类型数据是值类型

（2）引用类型：
    1、占用空间不固定，保存在堆中（当我们在程序中创建一个对象时，这个对象将被保存到运行时数据区中，以便反复利用（因为对象的创建成本通常较大），这个运行时数据区就是堆内存。堆内存中的对象不会随方法的结束而销毁，即使方法结束后，这个对象还可能被另一个引用变量所引用（方法的参数传递时很常见），则这个对象依然不会被销毁，只有当一个对象没有任何引用变量引用它时，系统的垃圾回收机制才会在核实的时候回收它。）
    2、保存与复制的是指向对象的一个指针
    3、使用instanceof检测数据类型
    4、使用new()方法构造出的对象是引用型
```

12.队列

13.原型

```
javascript中万物皆对象
函数对象 function[prototype]
普通对象 objects[__proto__]
```

14.javascript 设计模式

```
创建型


1.抽象工厂模式（Abstract Factory）

2.构建者模式（Builder）

3.工厂方法模式（Factory Method）

4.原型模式（Prototype）

5.单例模式（Singleton）


结构型：

1.适配器模式（Adapter）

2.桥接模式（Bridge）

3.组合模式（Compositor）

4.装饰者模式（Decorator）

5.外观模式（Facade）

6.享元模式（Flyweight）

7.代理模式（Proxy）


行为：

1.职责链模式（Chain of Responsibility）

2.命令模式（Command）

3.解释器模式（Interpreter）

4.迭代器模式（Iterator）

5.中介者模式（Mediator）

6.备忘录模式（Memento）

7.观察者模式（Observer）

8.状态模式（State）

9.策略模式（Strategy）

10.模板方法模式（Template Method）

11.访问者模式（Visitor）

```

15.前端如何对页面性能进行优化

```
1.减少http请求
2.减少http请求大小
3.将CSS或JavaScript放到外部文件中，避免使用<style>或者<script>标签直接引入（根据实际文件大小和业务场景来选择)
4.避免页面空的href和src
5.为HTML指定Cache-Control或Expires
6.合理设置Etag和Last-Modified
7.减少页面重定向
8.使用静态资源分域存放来增加下载并行数
9.使用静态资源CDN来存储文件
10.使用CDN Combo下载传输内容
11.使用可缓存的AJAX
12.使用GET来完成AJAX请求
13.减少Cookie的大小并进行Cookie隔离
14.缩小favicon.ico并缓存
15.推荐使用异步JavaScript资源
16.消除阻塞渲染的CSS及JavaScript
17.避免使用CSSimport引用加载CSS
```

16.前端内存泄漏

```
1.意外的全局变量
2.没有及时清理定时器和回调函数
3.脱离Dom的引用
4.闭包
5.echart不停调用
```

17.使用 typeof bar ===“object”来确定 bar 是否是一个对象时有什么潜在的缺陷？这个陷阱如何避免？

```
如果bar的值为null的话,这样的话判断依旧成立,故错误
1.bar是个函数
console.log((bar !== null) && ((typeof bar === "object") || (typeof bar === "function")));
2.bar是个数组
console.log((bar !== null) && (typeof bar === "object") && (toString.call(bar) !== "[object Array]"));

ES5
console.log(Array.isArray(bar));
```

18.

```
(function(){
var a = b = 3;
})();

console.log("a defined? " + (typeof a !== 'undefined'));
console.log("b defined? " + (typeof b !== 'undefined'));
```

以上代码输出?

```
var a=b=3我们拆解下
b=3
var a=b
此时b为全局变量,
如果在严格模式的话会报错,b未定义
```

19.

```
var myObject = {
    foo: "bar",
        func: function() {
        var self = this;
        console.log("outer func: this.foo = " + this.foo);
        console.log("outer func: self.foo = " + self.foo);
        (function() {
            console.log("inner func: this.foo = " + this.foo);
            console.log("inner func: self.foo = " + self.foo);
        }());
    }
};
myObject.func();
```

以上代码输入什么
```
outer func: this.foo = bar
outer func: self.foo = bar
inner func: this.foo = undefined
inner func: self.foo = bar
```
20.
```
function foo1(){
    return {
    bar: "hello"
    };
}

function foo2(){
    return
    {
    bar: "hello"
    };
}
console.log(foo1())
console.log(foo2())
```
以上代码会输入相同结果吗?
```
输入结果是不同的,
foo1()={bar="hello}
foo2()=undefined
原因与JavaScript中分号在技术上是可选的事实有关（尽管忽略它们通常是非常糟糕的形式）。因此，在foo2()中遇到包含return语句的行（没有其他内容）时，会在return语句之后立即自动插入分号。
```
21.什么是NaN
```
Number.isNaN()
```
22.
```
(function() {
    console.log(1);
    setTimeout(function(){console.log(2)}, 1000);
    setTimeout(function(){console.log(3)}, 0);
    console.log(4);
})();
```
执行以上代码,1-4如何排列
```
1,4,3,2
```
23.
```
console.log(1 + "2" + "2");
console.log(1 + +"2" + "2");
console.log(1 + -"1" + "2");
console.log(+"1" + "1" + "2");
console.log( "A" - "B" + "2");
console.log( "A" - "B" + 2);
```
执行以上代码会出现什么结果
```
"122"
"32"
"02"
"112"
"NaN2"
NaN

示例1：1 +“2”+“2”输出：“122”说明：第一个操作在1 +“2”中执行。由于其中一个操作数（“2”）是一个字符串，所以JavaScript假定需要执行字符串连接，因此将1的类型转换为“1”，1 +“2”转换为“12”。然后，“12”+“2”产生“122”。

示例2：1 + +“2”+“2”输出：“32”说明：根据操作顺序，要执行的第一个操作是+“2”（第一个“2”之前的额外+被视为一个一元运算符）。因此，JavaScript将“2”的类型转换为数字，然后将一元+符号应用于它（即将其视为正数）。结果，下一个操作现在是1 + 2，当然这会产生3.但是，我们有一个数字和一个字符串之间的操作（即3和“2”），所以JavaScript再次转换数值赋给一个字符串并执行字符串连接，产生“32”。

示例3：1 + - “1”+“2”输出：“02”说明：这里的解释与前面的示例相同，只是一元运算符是 - 而不是+。因此，“1”变为1，然后在应用 - 时将其变为-1，然后将其加1到产生0，然后转换为字符串并与最终的“2”操作数连接，产生“02”。

示例4：+“1”+“1”+“2”输出：“112”说明：尽管第一个“1”操作数是基于其前面的一元+运算符的数值类型转换的，当它与第二个“1”操作数连接在一起时返回一个字符串，然后与最终的“2”操作数连接，产生字符串“112”。

示例5：“A” - “B”+“2”输出：“NaN2”说明：由于 - 运算符不能应用于字符串，并且既不能将“A”也不能将“B”转换为数值， “ - ”B“产生NaN，然后与字符串”2“串联产生”NaN2“。

例6：“A” - “B”+2输出：NaN说明：在前面的例子中，“A” - “B”产生NaN。但是任何运算符应用于NaN和其他数字操作数仍然会产生NaN。
```
24.
```
var b = 1;
function outer(){
    var b = 2
    function inner(){
        b++;
        var b = 3;
        console.log(b)
    }
    inner();
}
outer();
```
以上代码输出什么
3

25.
```
var x = 21;
var girl = function () {
    console.log(x);
    var x = 20;
};
girl ();
```
以上代码输入什么?
undefined
为什么它不显示21的全局值？原因是当函数执行时，它检查是否存在本地x变量但尚未声明它，因此它不会查找全局变量。
26.
```
(function () {
    try {
        throw new Error();
    } catch (x) {
        var x = 1, y = 2;
        console.log(x);
    }
    console.log(x);
    console.log(y);
})();
```
以上代码输出什么
```
1
undefined
2
```
var语句被挂起（没有它们的值初始化）到它所属的全局或函数作用域的顶部，即使它位于with或catch块内。但是，错误的标识符只在catch块内部可见。它相当于：
```
(function () {
    var x, y; // outer and hoisted
        try {
            throw new Error();
        } catch (x /* inner */) {
            x = 1; // inner x, not the outer one
            y = 2; // there is only one y, which is in the outer scope
            console.log(x /* inner */);
        }
    console.log(x);
    console.log(y);
})();
```


