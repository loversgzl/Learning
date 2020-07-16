# Vue 基础语法

# 资料
[B 站视频-前端学习资料大总结（含面试题）](https://www.bilibili.com/video/BV18f4y1S7jn)  
[B 站-Vue.js 从入门到精通 20h]()  
[官方文档-重要](https://cn.vuejs.org/v2/guide/)  
[segmentFault](https://segmentfault.com/blog/vue-write)  
进阶：Directive 自定义指令、.sync 修饰符、

# 语法
注释：`<!--  -->`  
等号：`===`  
不等于：`!==`  
`<br>`：回车  
`console.log("打印日志")`：正常执行，如果执行到这一句，那么会显示在控制台（F12）里面，比 alert 显示的数据更清晰，类型更丰富  

# 子组件结构
**基本框架**  
`<style>`：定义了样式，基本对编码没影响  
`<template>`：用来编辑前端代码，template 模板标签里包含的内容是要渲染在页面的内容，是 XML 语法格式，不是 HTML 语法格式，XML 和 HTML 语法基本类似，但是 XML 比 HTML 更加严格些，如在闭合标签这一块，XML 严格要求单标签必须闭合，如 `<input/>`，但在 HTML 中，单标签可闭合可不闭合，且最新的语法和推荐的语法是不闭合。还有就是在 XML 中，当标签内的内容为空时，可直接闭合，如 `<div/>` ，但是在 HTML 中则无这种语法。由于 XML 更加严格，因此 Vue 为了能更好地写 * .vue 文件的编译器，于是规定 template 模板标签里的内容必须用 XML 语法。  
`<script>`：定义了所有数据和方法，逻辑处理一般在这里进行。  
****
**Vue(){}**  
**$**：表示定义的变量，与实例区分； 
**el**：用来将新建的 vue 绑定到对应的 ID 或者 class；el 属性又称挂载点，可认为是 element 的简写，创建一个 vue实例 得知道是在哪一块元素上创建 Vue实例 ，对哪一块视图进行操作；  
**data**：用来定义数据； 
**created**：初始化数据函数 ；
**mounted**：初始化页面元素； 
**computed**：计算属性，对数据进行一定的操作，可以包含逻辑处理操作（把变量当做函数处理并赋值）  
**watch**：监听属性； 
**methods**：用来定义方法； 


# 函数
* **问：Vue 的生命周期？**
* **答**：总共分为 8 个阶段创建前/后，载入前/后，更新前/后，销毁前/后：
**创建前/后**： 在 beforeCreate 阶段，Vue 实例的挂载元素 $el 和 **数据对象** data 都为 undefined，还未初始化。在 created 阶段，Vue 实例的数据对象 data 有了，$el 还没有；  
**载入前/后**：在 beforeMount 阶段，Vue 实例的 $el 和 data 都初始化了，但还是挂载之前为虚拟的DOM 节点，data.message 还未替换。在mounted阶段，Vue 实例挂载完成，data.message 成功渲染；  
**更新前/后**：当 data 变化时，会触发 beforeUpdate 和 updated 方法；  
**销毁前/后**：在执行 destroy 方法后，对 data 的改变不会再触发周期函数，说明此时 Vue 实例已经解除了事件监听以及和 DOM 的绑定，但是 DOM 结构依然存在。
****
* **问：vue 项目中看到 import 时出现 @ 符号？**
```js
  //这和 vue 没关系。./ 这是相对路径的意思。@/ 这是 webpack 设置的路径别名。这东西在 vue 标准模板里面的 build/webpack.base.conf 这个文件里面。
  resolve: {
    // 路径别名
    alias: {
    '@': resolve('src'),
    'vue$': 'vue/dist/vue.esm.js'
    }
  },//就是说@这东西代表着到 src 这个文件夹的路径
```
****
* **问：ref 和 $refs？**  
**vm.$refs**：类型 Object；只读；一个对象，持有已注册 ref 的所有子组件。相对 document.getElementById 的方法，会减少获取 Dom 节点的消耗。  
**ref**：预期 string；ref 被用来给元素或子组件注册引用信息。引用信息将会注册在父组件的 $refs 对象上。  
如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素；如果在子组件上，引用就指向组件实例；（注意关于 ref 注册时间的重要说明：因为 ref 本身是作为渲染结果被创建的，在初始渲染的时候你不能访问他们-他们还不存在！**$refs**：也不是响应式的，因此你不应该视图用它在模板中做数据绑定）
****
* **问：props:["property"] 外部数据？**
1. 注意：这是子组件才拥有的，定义自定义属性，用来方便父组件使用，并向子组件传递数据；  
2. 注意： "property" 是单向绑定的，当父组件（使用了该组件的组件）的属性变化时，将传导给子组件，但是不会反过来；  
3. "prop" 是子组件用来接受父组件传递过来的数据的一个自定义属性。父组件的数据需要通过 property 把数据传给子组件，子组件需要显式地用 props 选项声明 "property"；  
4. 动态 "property"：类似于用 v-bind 绑定 HTML 特性到一个表达式，也可以用 v-bind 动态绑定 props 的值到父组件的数据中。每当父组件的数据变化时，该变化也会传导给子组件；  
5. 父组件是使用 props 传递数据给子组件，但如果子组件要把数据传递回去，就需要使用自定义事件；  
****
* **问：如何传递 整数或者 boolean？**
* **答**：初学者常犯的一个错误是使用字面量语法传递数值，`<comp some-prop="1"></comp>`，因为它是一个字面 prop ，它的值以字符串 "1" 而不是以实际的数字传下去。如果想传递一个实际的 JavaScript 数字，需要使用 v-bind ，从而让它的值被当作 JavaScript 表达式计算：`<comp v-bind:some-prop="1"></comp>`
****
* **问：created(){}  钩子函数与 mounted(){} 钩子函数的使用区别？**  
[两者区别](https://blog.csdn.net/ygy211715/article/details/80079603)  
**一般 creadted 钩子函数主要是用来初始化数据**  
官方解释说 created 是在实例创建完成后被立即调用。在这一步，实例已完成以下配置：数据观测 (data observer)，属性和方法的运算，watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。  
这话的意思我觉得重点在于说**挂架阶段**还没开始，什么叫还没开始挂载，也就是说，模板还没有被渲染成html，也就是这时候通过 id 什么的去查找页面元素是找不到的。  

**mounted 钩子函数一般是用来向后端发起请求拿到数据以后做一些业务处理，Dom 操作一般是在 mounted 钩子函数中进行的**  
官方解释如下：el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。如果 root 实例挂载了一个文档内元素，当 mounted 被调用时 vm.$el 也在文档内；  
这意思是该钩子函数是在 挂载完成 以后也就是模板渲染完成以后才会被调用；  

**mounted**
[mounted 与 created 的区别](https://blog.csdn.net/ygy211715/article/details/80079603)  
在使用vue框架的过程中，我们经常需要给一些数据做一些初始化处理，这时候我们常用的就是在created与mounted选项中作出处理。  
如果在此阶段需要获取 html 的属性如 id或者class，则需要使用 mounted，因为该函数会在 html 模板加载完成后进行初始化，created 则不会。因此，Dom操作一般是在mounted钩子函数中进行的。  
****
* **问：vue 的 created 函数中，方法的执行顺序？**  
**情景**：vue框架中通常在created钩子函数里执行访问数据库的方法，然后返回数据给前端，前端data中定义全局变量接收数据。但是如果你在created中执行了好几个访问数据库的函数，如果对他们的执行顺序是有要求的，那么哪个会先返回，哪个会后返回呢？并不是谁在前谁就先返回，如果你这样想，并且在后执行的函数中对先执行的函数返回的数据进行操作，经常会报错，提示某些属性不存在，或未定义；  
**原因**：这是因为 js 中默认执行网络请求是异步的，他们会按顺序发出请求之后就不管了，谁先返回是不确定的，这样在加载数据的时候不会因为某个网络请求慢，而在一直等待那个请求，导致其他请求阻塞，效率，体验很差；  
**解决方法**：如果一次加载页面需要执行多个网络请求，并且对返回的数据顺序是有要求的，就用.then()函数，当这个函数执行完后再执行下个函数；  
****
* **问：watch:{} 方法？**  
* **答**：监听 data 中的属性，如果有改变，则会调用对应的方法。
****
* **问：computed:{} 方法？**  
computed:{}   计算属性，什么是计算属性呢，我个人理解就是对数据进行一定的操作，可以包含逻辑处理操作，对计算属性中的数据进行监控。计算属性是基于它的以来进行更新的，只有在相关依赖发生改变时侧能更新变化，以函数的形式返回结果。然后可以像绑定普通属性一样在模板中绑定计算属性。
****
* **问： methods:{} 方法？**  
**一个方法中调用另一个方法**  
1、需要添加 const me = this; 然后在函数内部使用 me.xxMethod();  
2、有时候需要使用这种方式（这里绑定了作用域）：this.$options.methods.test.bind(this)();  

**使用上面第一个方法调用其他函数是可以，但是在其他函数里如果有this，则作用域是上面这个函数的作用域，所以会报错，作用域找不到！**
[如何绑定作用域](https://www.cnblogs.com/amingxiansen/p/9541917.html)

* **问： computed vs methods？**
我们可以使用 methods 来替代 computed，效果上两个都是一样的，但是 computed 是基于它的依赖缓存，只有相关依赖发生改变时才会重新取值。而使用 methods ，在重新渲染的时候，函数总会重新调用执行。

# 生命周期
在 Vue 中，将每个状态转变点称之为钩子，如实例创建后，和实例创建前，则实例创建就是个钩子，对应前后两个阶段，即是 beforeCreate 实例创建前，和 created 实例创建后，以下都是成对出现的，因此只需记一个就行。  
该属性是一个函数，在其对应的时期被调用。created实例出现在内存中mounted实例出现在页面中updated实例更新了destroyed实例从页面和内存中消亡了  
更多见**常见面试题**


# 父子组件 传参与主动调取数据
[state 六种传值方式](https://www.cnblogs.com/ming1025/p/13073753.html)  
注意：$refs["ref"] 都只是 自己调用 自己的引用，然后进行处理，不会跨组件。  
****
**1.1 父组件 向 子组件 传参**
```js
// 1.父组件在引用子组件时添加属性
<Child  :childAttr = "fatherAttr"></Child>
// 2.子组件中使用如下获取数据
 props: ["childAttr"]
```
****
**1.2 父组件 调用 子组件 的值或方法**
：给子组件添加引用如， ref="form" ，然后使用该引用即可！相当于操作 DOM
```js
// 1. 父组件调用子组件的时候添加如下引用：
<Child ref="grid"></Child>
// 在父组件中通过如下方式即可，无需子组件参与：
this.$refs.grid.属性
this.$refs.grid.方法

//此方法其实适合于可以获取引用的任意控件，包括兄弟控件
// 2. 在父组件 调用子组件的方法时 同时传递参数：
me.$refs["grid"].$emit("search", me.Constant.FORM_MODEL_SEARCH);
// 子组件需要在 created 函数中添加如下接收函数：
me.$on('',function(val){ some Operation; })
```
****
**2.1 子组件 向父组件 传参**
[使用emit 向父组件传递参数](http://www.manongjc.com/article/115329.html)
```js
// 1. 子组件使用 emit 方式传递参数：
me.$emit('test', me.param)
// 父组件在引用子组件的时候，添加如下属性：
<Child @test="fatherMethod"></Child>
```
****
**2.2 子组件 调用父组件的数据和方法**
子组件通过$parent获取父组件的数据和方法，这种传值方式实际上类似于上边的属性传值中父组件给子组件的传递了子类对象this,只不过Vue官方给封装好了。
直接在子组件中使用this.$parent.XX，不需要做任何多余操作。
```js
me.$parent.属性
me.$parent.方法
```
****
**3.1 某个子组件获取另一个子组件的数据**
```js
// 1.需要父组件帮助，在公共父组件中：
<ChildOne ref="childOne"></ChildOne>
<ChildTwo :attr="$refs['childOne']"></ChildTwo>
//那么此时： ChildTwo 即可以像操作属性那样，轻松操作另一个组件中的数据了。
```
****
**3.2 某个子组件 向另一个子组件传值：使用 vuex 进行传值（又叫本地传值）**
```js
// 在 ChildOne 子组件中，先将数据放入 vuex 中：
me.$store.dispatch("rescueTeam/setSelectRescueTeam", data);
//在 ChildTwo 的 dialog 打开事件，将数据赋值到需要传递的 data 中即可：
me.formData = me.$store.state.rescueTeam.selectRescueTeam;
```
****
**3.3、某个子组件 向另一个子组件传值：使用全局 emit 传递（又叫通知传值(广播传值)不推荐-更改了根配置文件）**  
适用于父子组件、兄弟组件间进行传值：无关系组件不能用这种方式传值。(笔者理解是：对于上图中的A、B组件。假设A广播通知，B接收通知。挂载A的时候B卸载了，也就是说B被内存销毁了,B是接收不到广播的)
[eventBus 组件传值](https://jingyan.baidu.com/article/72ee561a09027be16138dfd5.html)
```js
// 在 main.js 中定义一个全局的 emit：
window.eventBus = new Vue(); 

//下面开始任意组件，跨域使用此方式传值，如果可以使用引用，则类似上面兄弟组件互相传值：
//传递值的组件在方法中写如下方法：
 eventBus.$emit('city', {cityName:params.name,cityCode:params.data.adcode}); 
 //接受值的组件在 created() 方法中写如下方法：获取到的 data 是一个对象
 eventBus.$on('city', (data)=>{}
```


# 与后台交互的方法
```js
//disRescueTeam Controller下的总目录，后端根路径
@RequestMapping("/disRescueTeam")

//前端：无参数调用方法
//后端：@GetMapping(value={"/listTeamType"})
let url = 'disRescueTeam/listTeamType';
me.$ajax.get(url).then(res => {});

//前端：新增某个用户，有参数列表
//后端：@PostMapping("/save")
me.$ajax.post("userManage/save", me.formData).then(res => {});


//前端：删除某个用户，有参数
//后端：@DeleteMapping("/{id}")
  me.$ajax.delete('disRescueTeam/' + data.id).then(res => {});

```

# V 开头的指令

**v-for**  
[参考博客](https://www.cnblogs.com/wangyfax/p/10073159.html)  
循环语句，注意一点，这里的键值对是反的 v-for = "(value, key ) in object";  
v-for循环的时候，key属性只能使用number或String。key在使用的时候，必须使用v-bind属性绑定的形式，指定key的值。在组件中使用v-for循环的时候，或者在一些特殊情况中，如果v-for有问题，必须在使用v-for的同时，指定唯一的 字符串/数字 类型 :key值。

```js
// v-if（条件判断）：用于判断是否显示该标签，true 为显示，false 为不显示
<div v-if="x > 0"> x大于0时展示的部分 </div>

// v-for（循环）：//一定要绑定个 key 属性，值为 key 不重复的值
<ul>
    <li v-for="(value,name) in users" :key="name">
        属性名:{{name}} 属性值：{{u.value}}
    </li>
</ul>

// v-show：表达式为真时显示该模块
<div v-show="表达式"> </div>
<div v-show="n%2 === 0"> n是偶数 </div>

// v-model：实现与 input（或者 select，radio） 进行双向数据绑定，这个 DOM 的 value 是什么，就把它赋值给绑定的变量。同理，变量的值与value的值相对应，那么就会选中哪个 DOM。
<input v-model="text"></input>

// v-on：绑定一个函数，在点击时会调用该函数 add()
<button v-on:click="add"> +1 </button> 
<button @click="add"> +1 </button> // 缩写
<button @click="add(1,3)"> 加法 </button> //传参函数，在点击时执行该函数 add(1,3)
<button @click="n+=1">将n+1</button> //表达式，在点击时执行表达式

// HTML 属性（如id，class等）中的值应使用 v-bind 指令。动态绑定自定义属性的值到父组件的数据中，每当父组件的数据变化时，该变化也会传导给子组件。
// v-bind：x 是变量，会被编译；v-bind: 的简写，直接在属性前加 :
<img v-bind:src="x"> </img> // 
<img :src="x"> </img>

// v-html：x的内容为 html 字符串，会被解析成对应的 html 元素
<div v-html="x"> </div>

//绑定对象：这里可以把 100px 简写成 100
<div :style="{border: '1px solid red', height: 100'}"></div>
```


* **问：（let，var，const） me =  this 对作用域的影响 ？ **  
不同作用域， this 所指代的对象不同。this 对象是在运行时基于函数的执行环境绑定的。  
在全局函数中（匿名函数具有全局性）：this 等于 window  
当函数被作为某个对象调用时：this 等于那个对象  
全局函数：data(),mounted(),methods,  
对象函数：通常在全局函数内部，this 即代表这个对象  

var me = this;   
//声明一个变量指向 Vue 实例 this，保证作用域一致。  
//me 只是一个变量名，this 代表父函数，如果在子函数还用 this，this 的指向就变成子函数了，me 就是用来存储指向的。  

* **问：var > let 的作用域？**  
[var 和 let 的区别](https://www.jianshu.com/p/84edd5cd93bd)  
let：只在代码块内部有效（即一个大括号内部，相当于 for 循环内的 int i）  
let：不允许在相同作用域内，重复声明同一个变量。  
let：一定要在声明后使用，否则报错。即不存在变量提升现象。  

var：使用 va r声明的变量，其作用域为该语句所在的函数内  
var：存在变量提升现象，可以在声明之前使用，值为 undefined。  

* **问：vuex 中dispatch 和 commit 的用法和区别？**  
dispatch：含有异步操作，例如向后台提交数据，写法： this.$store.dispatch('action方法名',值)  
commit：同步操作，写法：this.$store.commit('mutations方法名',值)  

* **问：Vue 里面的 slot 属性？**  
[深入理解VUE中的槽与槽范围](https://blog.csdn.net/weixin_41646716/article/details/80450873)



