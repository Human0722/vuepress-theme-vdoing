---
title: Vue2
categories: 
  - Vue
date: 2022-05-06 17:18:03
permalink: /pages/200f6b/
sidebar: auto
tags: 
  - 
author: 
  name: Xueliang
  link: https://github.com/Human0722
---

<span class="ec ec-nerd-face"></span><span class="ec ec-nerd-face"></span><span class="ec ec-nerd-face"></span> Vue.js笔记本

## vue-cli搭建vue项目基础框架。  

1. 安装vue
```shell
npm install -g @vue/cli
yarn global add @vue/cli
```
2. 创建项目
``` shell
vue create my-project
||
vue ui
```

## Vue对象

```javascript
<script>
  var app = new Vue({
    el: '#app', // 绑定容器
    data(): {   // 渲染的数据
      return {
        name: 'vue-project'
      }
    }
    methods: {  // 方法s

    },
    computed: {
      prices: function() {
        return 1+2;
      }
    },
    props: ['name'], // 接受属性值

    filters: {    // 格式化数据
      formateDate: function (value) {
        return 'value:' + value;
      }
    },
    mounted: {  //挂载前调用

    },
    beforeDestory: {  // 销毁前调用

    }

  });
</script>
```  

## 计算属性  
> 完成各种复杂逻辑操作，操作数据后返回一个结果。只要所操作数中的任一个被触发，就会重新执行计算属性。每个计算属性包含一个 getter 和 setter。修改数据触发 setter, 读取数据出发 getter。  

```javascript
<div>{% raw %}
  {{ prices }}
{% endraw %}</div>
var app = new Vue({
  el: '#app',
  computed: {
    prices: function () {
      return 1+2;
    }
  }
});
```  
计算属性和方法的区别就是：  计算属性有缓存，只有所依赖数据被改动则重新计算。而 methods 中的方法，只要是重新渲染就会被调用。选择取决于是否需要缓存~  

## class 与 style 的绑定   
> 常用的绑定样式的方法: 在标签上使用对象语法、数组语法快速绑定多个样式、内联样式

```javascript
// 对象语法
<div :class="{'active': isActive', 'border': isBorder}">Content</div>
new Vue({
  ...
  data() {
    return {
      isActive: true,
      isBorder: true
    }
  }
});

// 数组语法快速绑定多个样式
<div :class="[isActive, isBorder]">Content</div>
new Vue({
  ...
  data(): {
    return {
      isActive: 'active',
      isBorder: 'border',
    }
  }
});

// 通过计算属性绑定
<div :class="classes">Content</div>
new Vue({
  ...
  computed: {
    classes: function() {
      return {
        'active': true,
        'border': this.isBorder
      }
    }
  }
});

// 通过内联样式
<div :style="{ 'color': color, 'fontSize': fontSize + 'px'}">Content</div>
new Vue({
  ...
  data() {
    return {
      color: 'red',
      fontSize: 14
    }
  }
});
```  

## 常见的数组操作函数
> push pop shift unshift splice sort replace

```javascript
this.books.push({'xx': 'xx'});  //向数组 books 末尾追加
this.books.pop(); // 从数组 books 末尾弹出
this.books.shift(); // 从数组 books 头部弹出
this.books.unshift({'xx': 'xx'}); // 向数组 books 头部添加元素
this.books.splice(index, len, [item]);  //
  splice(1,1); // 删除第二个元素
  splice(1,1,'ttt');  // 替换第二个元素
  splice(1,0, 'ttt'); // 在第二个位置添加元素
this.books.sort();    // 神仙语法糖，还没搞懂
this.books.reverse(); // 将数组反转  
```  

## v-click 事件  

```javascript
// 简写
<button @click="xxx">按钮</button>   // 调用方法，计算属性...
<button @click="var++">按钮</button>  //直接执行 js 语句

// 传入参数
<button @click="xxx(10)">Button</button>
// 传入元素本身
<button @click="xxx(10,$event)"></button>
new Vue({
  ...
  xxx(num, event) {
    event.preventDefault();
  }
});

// 一些快捷修饰符: stop prevent, capture, self, once
<a @click.stop="handle"></a>    // 阻住默认跳转事件
<form @submit.prevent="handle"></form>    // 阻止表单默认提交事件
<input @keyup.enter="handle"></form>
```

## 表单与 v-model

```javascript
<input type="text" v-model.lazy="xxx">  // lost focus才会更新数据
<input type="text" v-model.number="xxx"> // 保存为数而非字符串
<input type="text" v-model.trim="xxx">  // 去除首尾的空格
```  


## 组件相关  

### 父组件通过 props 传值  

1.传递一个字符串

```javascript
// 父组件
<my-component name="myName"></my-component>
...
new Vue({
  component: {
    my-componet: require('./component/xxxComponent.vue').default;
  }
});

// 子组件中:
{% raw %}
<div>{{ name }}</div>
{% endraw %}
...
export default{
  props: ['name']
};

```  

2.传递一个对象   
>只要在父组件中属性名前面家上 : 即可。  

```javascript
<my-component :message="{name: 'xxx'}"></my-component>
```  

3.一些数据流  
当父组件传递属性值的时候，值会先存储在 props 列表中。此时的数据只能读，不能修改。  
如果数据是需要频繁修改的属性，则用 data(){} 转存，再操作 data 中的数据即可。 data 中的数据名称可以和 props 中的同名, 在末班中访问的时候会优先访问 data 中的数据。  

```javascript
// location: 子组件
<h3>{{ name }} </name>
export default{
  props: ['name'],
  data() {
    return {
      name: this.name
    }
  }
}
```  
如果数据是需要处理一次使用而无需多次更改， 这里使用 computed 来处理 props 属性值。但是 computed 属性名不能和 props 相同。
```javascript
// location: 子组件
export default {
  props: ['width'],
  computed: {
    style: function() {
      return {
        width: this.width + 'px'
      }
    }
  }
}
```  
4.数据过滤
> 主要是严格约束来自父组件的属性数据类型 ,数据类型有: String, Number, Bookean, Object, Array, Function   

```javascript
...
props: {
  propA: Number,
  PropB: [String, Number],
  PropC: {
    type: Boolean,
    default: true
  },
  PropD: {
    type: Number,
    required: true
  },
  PorpE: {
    type: Array,
    default: function() {
      return []
    }
  },
  propF: {
    validator: function(value) {
      return value > 10
    }
  }
}
```  

### 子组件向父组件通信  
> 核心思想就是，在子组件的方法中用 this.$emit('父组件监听事件名','数据'); 触发父组件的事件，然后父组件定义事件并接受数据。  

```javascript
// 子组件中(my-component):
<button @click="handleClick">+1</button>
export default {
  ...
  data() {
    return {
      conter: 0
    }
  },
  methods: {
    handleClick(){
      this.counter++;
      this.$emit('increase', this.counter);     // 触发父组件事件和传递数据
    }
  }
}

// 父组件中
<my-component @increase="handleIncrease"></my-component>    // 监听事件，不需要参数列表
...
new Vue({
  ...
  methods: {
    handleIncrease(num) {     // 定义事件，此处接收参数
      xxxx;
    }
  }
});
```  

如果来自子组件的数据是想更新父组件的数据，还有一种简便的方法。当在父组件上用 v-model 绑定数据时，在子组件中直接触发父组件的 input 事件即可，所有的数据更新流程已经在底层实现。

```javascript
// 父组件中
<my-component v-model="total"></my-component>
new Vue({
  data() {
    return {
      total: 0
    }
  }
});

// 子组件中
<button @click="handleClick">+1</button>
export default{
    data: function() {
        counter: 0
    }
    methods: {
      this.counter++;
      this.$emit('input', this.counter);
    }
};
```  
(不重要)如果上面的代码块难以理解， 可以通过下方的代码加深理解。  
```javascript
// 父组件
<my-component @input="handleInput"></my-component>
new Vue({
  data() {
    return {
      total: 0
    }
  },
  methods: {
    handleInput(num) {
      this.total = num;
    }
  }
});

// 子组件中
<button @click="handleClick">+1</button>
export default {
  methods: {
    handleClick: function() {
      this.$emit('input', 8)
    }
  }
}
```

### 非父子组件通信  

Case1: 可以通过新建一个 Vue 实例作为数据中继。具体实现如下: 先新建一个 Vue 实例 bus,在注册 app容器（父组件） 的时候注册监听 bus 的事件,然后在子组件中触发 bus 中的事件，来触发父组件中的监听事件。
``` javascript
// 父组件中
<div id="app">
  {{ message }}
  <my-component></my-component>
</div>
<script>
    var bus = new Vue();
    new Vue({
        data: {
          message: ''
        },
        mounted: function() {
          var _this = this;
          // 监听来自 bus 实例的事件
          bus.$on('on-message', function(msg) {   //这里监听
              _this.message = msg;
          });
        }
    });

// 子组件中
    <button @click="handleEvent">传递事件</button>
    export default {
      methods: {
        handleEvent: function() {
          bus.$emit('on-message', '来自 my-component 的数据');   //这里触发
        }
      }
    }
</script>
```  

Case2: 父链：在子组件中使用 this.$parent 可以直接访问父组件。  

```javascript
// 父组件
{% raw %}{{ message }}{% endraw %}
<component-a></component-a>
new Vue({
  data() {
    return {
      message: ''
    }
  }
});

// 子组件
<button @click="handleEvent">修改父组件属性值</button>
export default{
  methods: {
    handleEvent: function() {
      this.$parent.message = "来自自组件的数据"
    }
  }
}
```  

Case3: 子链【xxxxxx不看也罢】  

### slot 插槽  
> 可以简单理解成从父元素传递 DOM 元素给子元素的一种技术。  

```javascript
// 子组件： my-component
<div class="abc">
  <slot name="xxx"> // 通过 name 标记插槽, 如果没有 name, 会自动标记为默认插槽。
      Content...    // 默认内容，如果 父元素没有传递 DOM 来渲染这个
  </slot>    
</div>
// 父组件
<my-component>
    <p slot="xxx">    // 与子元素的 name 属性相对应
       ...
    </p>
</my-component>
```  

作用域插槽: 父元素传递的 Dom 给子元素的数据 留有占位符。

```javascript
// 子组件: my-component
<div>
  <slot msg="来自子组件的数据"></slot>
</div>
// 父组件
<my-component>
    <template slot-scope="props">
        <p>父元素的内容</p>
        <p>{%raw%}{{ props.msg }}{%endraw%}</p>
    </template>
</my-component>
```  

下面这种用法，先通过属性 props 父组件给子组件传递了一个数组，然后子组件又将每一个数据返给父组件。父组件又用返回的数据去渲染每一个插槽......
```javascript
<div>
    <my-component :books="books">
      <template slot="book" slot-scope="props">
          <li> {%raw%}{{ props.bookName }}{%endraw%}</li>
      </temlate>
    </my-component>
</div>
<script>
    Vue.component('my-component', {
      props: {
        books: [
          type: Array,
          default: function(){
            return [];
          }
        ]
      },
      template: '\
      <ul>\
      <slot name="book"\
      v-for="book in books"\
      :bookName="book.name">\
      </slot>\
      </ul>'
    });

    var app = new Vue({
      ...
      data() {
        return {
          books: [
            { name: '<xxx>'},
            { name: '<sss>'}
          ]
        }
      }
    });
</script>
```  

## Vue-router  
> 在前段实现 路由功能。可以按需加载~

1.安装和导入  

```shell
// 安装
npm install --save vue-router
yarn add vue-router
// 引用  
import Vue from 'vue'   // 定位
import VueRouter from 'vue-router'
import App from './App.vue'

Vue.use(VueRouter);   // 在 new Vue之前
```

2.基本使用  

```javascript
const Routers = [
  {
      path:   '/index',
      meta: {
        title: '首页'
      },
      component: (resolve) => require(['./component/index.vue'], resolve)     // 这种写法是按需加载路由:会将js 文件分成多个文件，配合每一个组件
  },
  {
      path:   '/about',
      meta: {
        title: '关于'
      },
      component: require('./component/about.vue')       // 这种写法是一次性加载所有路由
  }
]

// 一些路由配置
const RouterConfig = {
  mode: 'history',
  routes: Routers
}

// 生成路由对象
const router = new VueRouter(RouterConfig);
// 一些钩子
router.beforeEach((to, from, next) => {
    // to: 目标路由对象
    // from: 当前路由对象
    // next: 调用改方法后才会进入下一个钩子 ~ return
    window.document.title = to.meta.title;
    next();
});

router.afterEach((to, from, next) => {
  ...
});
// 实例化 Vue 时候加入 Router
new Vue({
  el: '#app',
  ...
  router: router
});

// 在模板中使用: 只能在 #app 容器中使用
<div id="app">
    <router-view></router-view>
    <router-link to="/index">首页</router-link>     // -> <a href="/index">首页</a>
    <router-link to="/index" tag="li">首页</router-link> //
    <router-link to="/index" replace>首页</router-link>   // 无痕模式，浏览器无法返回
</div>
```  

## Vuex
> 从组建中分离，几种管理易于共享的数据管理。  

1.安装和引用
```javascript
// 安装 yarn add vuex
// 引入
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
...

const store = new Vuex.Store({
  key: value
});

new Vue({
  el: '#app',
  ...
  store: store
});
```  

2.直接读数据  
> state 中的数据可以直接读取，但是不能直接修改！

```javascript
const store = new Vuex.Store({
  state: {
    count01: 0,
    count02: 3
  }
});
// 组件中
<div>
{%raw%}
  {{ this.$store.state.count01}}
  {{ count02 }}
{%endraw%}
</div>
...
export default {
  computed: {
    count02() {
      return this.$store.state.count
    }
  }
}
```  

3. 通过 mutations 来修改数据  
> 直接定义一些简单的数据操作逻辑，更复杂的数据操作逻辑用

```javascript
const store = new Vuex.Store({
    state: {
        count: 0
    },
    mutataions: {
        increment(state) {
            state.count++;
        },
        decrement(state, n=2) {
            state.count += n
        }
    }
});

// 组件中
<button @handleIncrement>Add</button>
<button @handleDecrement>Dele</button>
...
export default {
   methods: {
      increment() {
          this.$store.commit('increment')
      },
      decrement() {
        this.$store.commit('decrement', 5)
      }
   }
}
```  

``` mutations ``` 中不要进行复杂的操作，比如异步等等。这些要在 ``` action ```中进行。

4.用 getters 进行数据读前再操作(过滤、统计、排序)  
> 如果说 state 是从组件的 data 模块分离出来，那么 getter 就是从 computed 中分离出来的~

```javascript
const store = new Vuex.Store({
    state: {
        list: [1, 5, 9, 11]
    },
    getters: {
        filteredList: (state) => {
            return state.list.filter((item) => item < 10));
        },
        listCount:  (state, getters) => {
            return getters.filteredList.length;
        }
    }
});

// 组件中
<div>{%raw%}{{ list }}{%endraw%}</div>
...
export default{
    computed: {
       list: function() {
         return this.$store.getters.filteredList
       },

       listCount: function() {
         return this.$store.getters.listCount
       }
    }
}
```  

5.在 action 中完成复杂的数据操作

```javascript
// main.js  
const store = new Vuex.Store({
    state: {
      count: 0
    },

    mutataions: {
      increment(state, n=1) {
         state.count += n;
      }
    },
    actions: {
      asyncIncrement (context) {
        return new Promise( resolve => {
          setTimeout(()=>{
            context.commit('increment');
            resolve();
          },1000);
        });
      }
    }
});

// 组件中
<button @click="@handleAsyncIncrease">+1</button>
...
export default{
    ...
    methods: {
        handleAsyncIncrease() {
            this.$store.dispatch('asyncIncrement').then( ()=>{
                console.log(this.$store.state.count)
            });
        }
    }
}
```  

```mutations``` 和 ```action```看起来很相似，规定： 设计数据改变的，用mutations, 有业务逻辑的使用 actions.  


6.利用 module 对数据再分组  
> 当项目很大时，store 里面的state、getters、mutations 会非常多,都放在 main.js 中显得很不友好, 使用 modules 可以把他们写到不同的文件中。每个 module 都拥有自己的state、getters、mutations、actions.

```javascript
    const moduleA = {
        state: {},
        mutations: {},
        actions: {},
        getters: {}      
    }

    const moduleB = {
        state: {}
        ....    
    }

    ...

    const store = new Vuex.Store({
        modules: {
            a: moduleA,
            b: mouduleB
        }
    });

    ...

    store.state.a     // moduleA 的状态
    store.state.b     // moduleB 的状态
```  


## 最佳实践
