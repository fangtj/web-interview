# Vue

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
v-if v-show v-bind(:) v-for v-model  v-text v-html v-on(@)
```
12.MVVM
```
MVVM的设计思想：关注Model（数据）的变化，让MVVM框架去自动更新DOM的状态，比较主流的实现有：angular的（脏值检测）vue的（数据劫持->发布订阅模式）我们重点了解vue（数据劫持->发布订阅模式）的实现方式，让我们从操作DOM的繁琐操作中解脱出来
```
13.nextTick

在下次dom更新循环结束之后执行延迟回调，可用于获取更新后的dom状态


- 新版本中默认是mincrotasks, v-on中会使用macrotasks


- macrotasks任务的实现:

    - setImmediate / MessageChannel / setTimeout

14.生命周期
- _init_

    - initLifecycle/Event，往vm上挂载各种属性
    - callHook: beforeCreated: 实例刚创建
    - initInjection/initState: 初始化注入和 data 响应性
    - created: 创建完成，属性已经绑定， 但还未生成真实dom
    - 进行元素的挂载： $el / vm.$mount()
    - 是否有template: 解析成render function

        - *.vue文件: vue-loader会将<template>编译成render function
        
    - beforeMount: 模板编译/挂载之前
    - 执行render function，生成真实的dom，并替换到dom tree中
    - mounted: 组件已挂载



- update:

    - 执行diff算法，比对改变是否需要触发UI更新
    - flushScheduleQueue

        - watcher.before: 触发beforeUpdate钩子		- watcher.run(): 执行watcher中的 notify，通知所有依赖项更新UI
    - 触发updated钩子: 组件已更新



- actived / deactivated(keep-alive): 不销毁，缓存，组件激活与失活

- destroy:

    - beforeDestroy: 销毁开始
    - 销毁自身且递归销毁子组件以及事件监听
        - remove(): 删除节点
        - watcher.teardown(): 清空依赖
        - vm.$off(): 解绑监听
        
    - destroyed: 完成后触发钩子

```

new Vue({})

// 初始化Vue实例
function _init() {
	 // 挂载属性
    initLifeCycle(vm) 
    // 初始化事件系统，钩子函数等
    initEvent(vm) 
    // 编译slot、vnode
    initRender(vm) 
    // 触发钩子
    callHook(vm, 'beforeCreate')
    // 添加inject功能
    initInjection(vm)
    // 完成数据响应性 props/data/watch/computed/methods
    initState(vm)
    // 添加 provide 功能
    initProvide(vm)
    // 触发钩子
    callHook(vm, 'created')
		
	 // 挂载节点
    if (vm.$options.el) {
        vm.$mount(vm.$options.el)
    }
}

// 挂载节点实现
function mountComponent(vm) {
	 // 获取 render function
    if (!this.options.render) {
        // template to render
        // Vue.compile = compileToFunctions
        let { render } = compileToFunctions() 
        this.options.render = render
    }
    // 触发钩子
    callHook('beforeMounte')
    // 初始化观察者
    // render 渲染 vdom， 
    vdom = vm.render()
    // update: 根据 diff 出的 patchs 挂载成真实的 dom 
    vm._update(vdom)
    // 触发钩子  
    callHook(vm, 'mounted')
}

// 更新节点实现
funtion queueWatcher(watcher) {
	nextTick(flushScheduleQueue)
}

// 清空队列
function flushScheduleQueue() {
	 // 遍历队列中所有修改
    for(){
	    // beforeUpdate
        watcher.before()
         
        // 依赖局部更新节点
        watcher.update() 
        callHook('updated')
    }
}

// 销毁实例实现
Vue.prototype.$destory = function() {
	 // 触发钩子
    callHook(vm, 'beforeDestory')
    // 自身及子节点
    remove() 
    // 删除依赖
    watcher.teardown() 
    // 删除监听
    vm.$off() 
    // 触发钩子
    callHook(vm, 'destoryed')
}
```
15.数据响应(数据劫持)
看完生命周期后，里面的watcher等内容其实是数据响应中的一部分。数据响应的实现由两部分构成: 观察者( watcher ) 和 依赖收集器( Dep )，其核心是 defineProperty这个方法，它可以 重写属性的 get 与 set 方法，从而完成监听数据的改变。

- Observe (观察者)观察 props 与 state
    - 遍历 props 与 state，对每个属性创建独立的监听器( watcher )


- 使用 defineProperty 重写每个属性的 get/set(defineReactive）

    - get: 收集依赖
        - Dep.depend()
            - watcher.addDep()
    - set: 派发更新
        - Dep.notify()
        - watcher.update()
        - queenWatcher()
        - nextTick
        - flushScheduleQueue
        - watcher.run()
        - updateComponent()

```
let data = {a: 1}
// 数据响应性
observe(data)

// 初始化观察者
new Watcher(data, 'name', updateComponent)
data.a = 2

// 简单表示用于数据更新后的操作
function updateComponent() {
    vm._update() // patchs
}

// 监视对象
function observe(obj) {
	 // 遍历对象，使用 get/set 重新定义对象的每个属性值
    Object.keys(obj).map(key => {
        defineReactive(obj, key, obj[key])
    })
}

function defineReactive(obj, k, v) {
    // 递归子属性
    if (type(v) == 'object') observe(v)
    
    // 新建依赖收集器
    let dep = new Dep()
    // 定义get/set
    Object.defineProperty(obj, k, {
        enumerable: true,
        configurable: true,
        get: function reactiveGetter() {
        	  // 当有获取该属性时，证明依赖于该对象，因此被添加进收集器中
            if (Dep.target) {
                dep.addSub(Dep.target)
            }
            return v
        },
        // 重新设置值时，触发收集器的通知机制
        set: function reactiveSetter(nV) {
            v = nV
            dep.nofify()
        },
    })
}

// 依赖收集器
class Dep {
    constructor() {
        this.subs = []
    }
    addSub(sub) {
        this.subs.push(sub)
    }
    notify() {
        this.subs.map(sub => {
            sub.update()
        })
    }
}

Dep.target = null

// 观察者
class Watcher {
    constructor(obj, key, cb) {
        Dep.target = this
        this.cb = cb
        this.obj = obj
        this.key = key
        this.value = obj[key]
        Dep.target = null
    }
    addDep(Dep) {
        Dep.addSub(this)
    }
    update() {
        this.value = this.obj[this.key]
        this.cb(this.value)
    }
    before() {
        callHook('beforeUpdate')
    }
}

```
16.virtual dom 原理实现

- 创建 dom 树


- 树的diff，同层对比，输出patchs(listDiff/diffChildren/diffProps)
    - 没有新的节点，返回
    - 
    - 新的节点tagName与key不变， 对比props，继续递归遍历子树
        - 对比属性(对比新旧属性列表):
            - 旧属性是否存在与新属性列表中
            - 都存在的是否有变化
            - 是否出现旧列表中没有的新属性
            
        - tagName和key值变化了，则直接替换成新节点



- 渲染差异

    - 遍历patchs， 把需要更改的节点取出来
    - 局部更新dom

```
// diff算法的实现
function diff(oldTree, newTree) {
	 // 差异收集
    let pathchs = {}
    dfs(oldTree, newTree, 0, pathchs)
    return pathchs
}

function dfs(oldNode, newNode, index, pathchs) {
    let curPathchs = []
    if (newNode) {
        // 当新旧节点的 tagName 和 key 值完全一致时
        if (oldNode.tagName === newNode.tagName && oldNode.key === newNode.key) {
        	  // 继续比对属性差异
            let props = diffProps(oldNode.props, newNode.props)
            curPathchs.push({ type: 'changeProps', props })
            // 递归进入下一层级的比较
            diffChildrens(oldNode.children, newNode.children, index, pathchs)
        } else {
        	  // 当 tagName 或者 key 修改了后，表示已经是全新节点，无需再比
            curPathchs.push({ type: 'replaceNode', node: newNode })
        }
    }

	 // 构建出整颗差异树
    if (curPathchs.length) {
    		if(pathchs[index]){
    			pathchs[index] = pathchs[index].concat(curPathchs)
    		} else {
    			pathchs[index] = curPathchs
    		}
    }
}

// 属性对比实现
function diffProps(oldProps, newProps) {
    let propsPathchs = []
    // 遍历新旧属性列表
    // 查找删除项
    // 查找修改项
    // 查找新增项
    forin(olaProps, (k, v) => {
        if (!newProps.hasOwnProperty(k)) {
            propsPathchs.push({ type: 'remove', prop: k })
        } else {
            if (v !== newProps[k]) {
                propsPathchs.push({ type: 'change', prop: k , value: newProps[k] })
            }
        }
    })
    forin(newProps, (k, v) => {
        if (!oldProps.hasOwnProperty(k)) {
            propsPathchs.push({ type: 'add', prop: k, value: v })
        }
    })
    return propsPathchs
}

// 对比子级差异
function diffChildrens(oldChild, newChild, index, pathchs) {
		// 标记子级的删除/新增/移动
    let { change, list } = diffList(oldChild, newChild, index, pathchs)
    if (change.length) {
        if (pathchs[index]) {
            pathchs[index] = pathchs[index].concat(change)
        } else {
            pathchs[index] = change
        }
    }

	 // 根据 key 获取原本匹配的节点，进一步递归从头开始对比
    oldChild.map((item, i) => {
        let keyIndex = list.indexOf(item.key)
        if (keyIndex) {
            let node = newChild[keyIndex]
            // 进一步递归对比
            dfs(item, node, index, pathchs)
        }
    })
}

// 列表对比，主要也是根据 key 值查找匹配项
// 对比出新旧列表的新增/删除/移动
function diffList(oldList, newList, index, pathchs) {
    let change = []
    let list = []
    const newKeys = getKey(newList)
    oldList.map(v => {
        if (newKeys.indexOf(v.key) > -1) {
            list.push(v.key)
        } else {
            list.push(null)
        }
    })

    // 标记删除
    for (let i = list.length - 1; i>= 0; i--) {
        if (!list[i]) {
            list.splice(i, 1)
            change.push({ type: 'remove', index: i })
        }
    }

    // 标记新增和移动
    newList.map((item, i) => {
        const key = item.key
        const index = list.indexOf(key)
        if (index === -1 || key == null) {
            // 新增
            change.push({ type: 'add', node: item, index: i })
            list.splice(i, 0, key)
        } else {
            // 移动
            if (index !== i) {
                change.push({
                    type: 'move',
                    form: index,
                    to: i,
                })
                move(list, index, i)
            }
        }
    })

    return { change, list }
}

```
17.Proxy 相比于 defineProperty 的优势
- 数组变化也能监听到
- 不需要深度遍历监听




