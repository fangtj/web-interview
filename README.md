# web-interview
前端面试常见问题集锦
Javascript
1.前端安全,理解xss和csrf
```
XSS：跨站脚本（Cross-site scripting，通常简称为XSS）是一种网站应用程序的安全漏洞攻击，是代码注入的一种。它允许恶意用户将代码注入到网页上，其他用户在观看网页时就会受到影响。这类攻击通常包含了HTML以及用户端脚本语言。
CSRF:跨站请求伪造（Cross-site request forgery），也被称为 one-click attack 或者 session riding，通常缩写为 CSRF 或者 XSRF， 是一种挟制用户在当前已登录的Web应用程序上执行非本意的操作的攻击方法。
说的直白一点儿
XSS： 通过客户端脚本语言（最常见如：JavaScript）在一个论坛发帖中发布一段恶意的JavaScript代码就是脚本注入，如果这个代码内容有请求外部服务器，那么就叫做XSS！
通常来说CSRF是由XSS实现的，所以CSRF时常也被称为XSRF [用XSS的方式实现伪造请求]（但实现的方式绝不止一种，还可以直接通过命令行模式（命令行敲命令来发起请求）直接伪造请求[只要通过合法验证即可]）。
XSS更偏向于代码实现（即写一段拥有跨站请求功能的JavaScript脚本注入到一条帖子里，然后有用户访问了这个帖子，这就算是中了XSS攻击了），CSRF更偏向于一个攻击结果，只要发起了冒牌请求那么就算是CSRF了。
```
2.一次完整的HTTP事务是怎样的
```
a. 域名解析
b. 发起TCP的3次握手
c. 建立TCP连接后发起http请求
d. 服务器端响应http请求，浏览器得到html代码
e. 浏览器解析html代码，并请求html代码中的资源
f. 浏览器对页面进行渲染呈现给用户
```
3.数组快速排序
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
4.cookie,sessionStorage和localStorage的区别
```
共同点：都是保存在浏览器端，且同源的。
区别：cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递。而sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下。存储大小限制也不同，cookie数据不能超过4k，同时因为每次http请求都会携带cookie，所以cookie只适合保存很小的数据，如会话标识。sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。数据有效期不同，
sessionStorage：仅在当前浏览器窗口关闭前有效，自然也就不可能持久保持；
localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；
cookie只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。作用域不同，sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localStorage 在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的。Web Storage 支持事件通知机制，可以将数据更新的通知发送给监听者。Web Storage 的 api 接口使用更方便
```
5.javascript中==和===的区别
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
6.闭包
```
 闭包是指在 JavaScript 中，内部函数总是可以访问其所在的外部函数中声明的参数和变量，即使在其外部函数被返回了之后。然后闭包可以把一个局部变量传递到外部供其他函数或是变量使用,也可以把一个变量长时间的保留在系统的内存中
简单的说就是提升作用域
闭包的缺点： 
1 更多的内存消耗 
2 性能问题（跨作用域访问） 
3滥用闭包函数会造成内存泄露，因为闭包中引用到的包裹函数中定义的变量都永远不会被释放，所以我们应该在必要的时候，及时释放这个闭包函数
闭包优点:
a. JavaScript允许你使用在当前函数以外定义的变量 
b. 即使外部函数已经返回，当前函数仍然可以引用在外部函数所定义的变量 
c. 闭包可以更新外部变量的值 
d. 用闭包模拟私有方法
```
7.哪些操作会导致内存泄漏
```
1. setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏。
2. 闭包
3. 控制台日志
4. 循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）
```
8.面向对象过程的三大基本特征
```
继承,多态,封装
```


Vue
1.Vue的MVVM
```
MVVM全称是Model-View-ViewModel,Vue是以数据驱动的,一旦dom创建,数据更新dom也就跟着更新
1、M就是Model模型层，存的一个数据对象。
2、V就是View视图层，所有的html节点在这一层。
3、VM就是ViewModel，它通过data属性连接Model模型层，通过el属性连接View视图层
```
2.keep-live
```
把切换出去的组件保留在缓存中，可以保留组件的状态或者避免重新渲染
```
3.组件之间通讯
```
prop  $emit
同级组件之间通讯vuex
```
4.vue生命周期的理解
```
创建前/后，载入前/后，更新前/后，销毁前/后
它的生命周期中有多个事件钩子，让我们在控制整个Vue实例的过程时更容易形成好的逻辑。
```
5.vue响应式原理


6.vuex几大属性
```
有五种，分别是 State、 Getter、Mutation 、Action、 Module
```
7.state
```
1、Vuex就是一个仓库，仓库里面放了很多对象。其中state就是数据源存放地，对应于与一般Vue对象里面的data
2、state里面存放的数据是响应式的，Vue组件从store中读取数据，若是store中的数据发生改变，依赖这个数据的组件也会发生更新
3、它通过mapState把全局的 state 和 getters 映射到当前组件的 computed 计算属性中
```
8.getter
```
1、getters 可以对State进行计算操作，它就是Store的计算属性
2、 虽然在组件内也可以做计算属性，但是getters 可以在多组件之间复用
3、 如果一个状态只在一个组件内使用，是可以不用getters
```
9.mutation
```
1、Action 类似于 mutation，不同在于：
2、Action 提交的是 mutation，而不是直接变更状态。
3、Action 可以包含任意异步操作
```
10.vue优点
```
低耦合。视图（View）可以独立于Model变化和修改，一个ViewModel可以绑定到不同的"View"上，当View变化的时候Model可以不变，当Model变化的时候View也可以不变。
可重用性。你可以把一些视图逻辑放在一个ViewModel里面，让很多view重用这段视图逻辑。
独立开发。开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计，使用Expression Blend可以很容易设计界面并生成xml代码。
```








.