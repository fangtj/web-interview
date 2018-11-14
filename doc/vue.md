#Vue

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
```
const Observer = function(data) {
  for (let key in data) {
    defineReactive(data, key);
  }
}
 
const defineReactive = function(obj, key) {
  const dep = new Dep();
  let val = obj[key];
  Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true,
    get() {
      console.log('in get');
      dep.depend();
      return val;
    },
    set(newVal) {
      if (newVal === val) {
        return;
      }
      val = newVal;
      dep.notify();
    }
  });
}
 
const observe = function(data) {
  return new Observer(data);
}
 
const Vue = function(options) {
  const self = this;
  if (options && typeof options.data === 'function') {
    this._data = options.data.apply(this);
  }
 
  this.mount = function() {
    new Watcher(self, self.render);
  }
 
  this.render = function() {
    with(self) {
      _data.text;
    }
  }
 
  observe(this._data);  
}
 
const Watcher = function(vm, fn) {
  const self = this;
  this.vm = vm;
  Dep.target = this;
  
  this.addDep = function(dep) {
    dep.addSub(self);
  }
 
  this.update = function() {
    console.log('in watcher update');
    fn();
  }
 
  this.value = fn();
  Dep.target = null;
}
 
const Dep = function() {
  const self = this;
  this.target = null;
  this.subs = [];
  this.depend = function() {
    if (Dep.target) {
      Dep.target.addDep(self);
    }
  }
 
  this.addSub = function(watcher) {
    self.subs.push(watcher);
  }
 
  this.notify = function() {
    for (let i = 0; i < self.subs.length; i += 1) {
      self.subs[i].update();
    }
  }
}
 
const vue = new Vue({
  data() {
    return {
      text: 'hello world'
    };
  }
})
 
vue.mount(); // in get
vue._data.text = '123'; // in watcher update /n in get

1.vue将data初始化为一个Observer并对对象中的每个值，重写了其中的get、set，data中的每个key，都有一个独立的依赖收集器。
2.在get中，向依赖收集器添加了监听
3.在mount时，实例了一个Watcher，将收集器的目标指向了当前Watcher
4.在data值发生变更时，触发set，触发了依赖收集器中的所有监听的更新，来触发Watcher.update
```

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
11.vue常用指令
```
v-if v-show v-bind v-for v-model 
```