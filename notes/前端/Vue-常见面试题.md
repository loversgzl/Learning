# Vue 常见面试题
[知乎摘录](https://zhuanlan.zhihu.com/p/92407628)
- <a href="#Vue 基础">Vue 基础</a>
- <a href="#语法">语法</a>
  - <a href="#指令">指令</a>
- <a href="#Vue-cli ">Vue-cli </a>
- <a href="#生命周期面试题">生命周期面试题</a>

<a name="Vue 基础"></a>
# Vue 基础

Vue 优点？
答：轻量级框架、双向数据绑定、组件化、

Vue 的两个核心点
答：数据驱动、组件系统
数据驱动：ViewModel，保证数据和视图的一致性。
组件系统：应用类 UI 可以看作全部是由组件树构成的。

Vue 和 jQuery 的区别
答：jQuery是使用选择器（$）选取 DOM 对象，对其进行赋值、取值、事件绑定等操作，其实和原生的 HTML 的区别只在于可以更方便的选取和操作 DOM 对象，而数据和界面是在一起的。比如需要获取 label 标签的内容：$("lable").val();，它还是依赖 DOM 元素的值。Vue 则是通过 Vue 对象将数据和 View 完全分离开来了。对数据进行操作不再需要引用相应的 DOM 对象，可以说数据和 View是分离的，他们通过 Vue 对象这个 vm 实现相互的绑定。这就是传说中的 MVVM。

渐进式框架的理解
答：主张最少；可以根据不同的需求选择不同的层级；

单页面应用和多页面应用区别及优缺点
答：单页面应用（SPA），通俗一点说就是指只有一个主页面的应用，浏览器一开始要加载所有必须的 html, js, css。所有的页面内容都包含在这个所谓的主页面中。但在写的时候，还是会分开写（页面片段），然后在交互的时候由路由程序动态载入，单页面的页面跳转，仅刷新局部资源。多应用于pc端。
多页面（MPA），就是指一个应用中有多个页面，页面跳转时是整页刷新单页面的优点：用户体验好，快，内容的改变不需要重新加载整个页面，基于这一点 spa 对服务器压力较小；前后端分离；页面效果会比较炫酷（比如切换页面内容时的专场动画）。单页面缺点：不利于 seo；导航不可用，如果一定要导航需要自行实现前进、后退。（由于是单页面不能用浏览器的前进后退功能，所以需要自己建立堆栈管理）；初次加载时耗时多；页面复杂度提高很多。

请说下封装 Vue 组件的过程？
答：1. 建立组件的模板，先把架子搭起来，写写样式，考虑好组件的基本逻辑。(os：思考1小时，码码10分钟，程序猿的准则。)　　
2. 准备好组件的数据输入。即分析好逻辑，定好 props 里面的数据、类型。　　
3. 准备好组件的数据输出。即根据组件逻辑，做好要暴露出来的方法。　　
4. 封装完毕了，直接调用即可

40.Vue常用的UI组件库
答：Mint UI，element，VUX

<a name="语法"></a>
# 语法

Vue 父子组件传递数据的方式？
答：六种

如何让 CSS 只在当前组件中起作用？
答：在组件中的 style 前面加上 scoped

如何获取 DOM?
答：ref="domName" 用法：this.$refs.domName

引进组件的步骤
答: 在template中引入组件；在script的第一行用import引入路径；用component中写上组件名称。

为什么使用 key?
答：需要使用 key 来给每个节点做一个唯一标识，Diff 算法就可以正确的识别此节点。作用主要是为了高效的更新虚拟 DOM。

分别简述 computed 和 watch 的使用场景
答：computed：当一个属性受多个属性影响的时候就需要用到 computed，最典型的栗子： 购物车商品结算的时候
watch：当一条数据影响多条数据的时候就需要用 watch，栗子：搜索数据

**5、<keep-alive></keep-alive>的作用是什么?**
答：keep-alive 是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。

**$nextTick的使用**
答：当你修改了 data 的值然后马上获取这个 DOM 元素的值，是不能获取到更新后的值，你需要使用 $nextTick 这个回调，让修改后的 data 值渲染更新到 DOM 元素之后在获取，才能成功。

**Vue组件中data为什么必须是一个函数？**
答：因为JavaScript的特性所导致，在component中，data必须以函数的形式存在，不可以是对象。　　组建中的data写成一个函数，数据以函数返回值的形式定义，这样每次复用组件的时候，都会返回一份新的data，相当于每个组件实例都有自己私有的数据空间，它们只负责各自维护的数据，不会造成混乱。而单纯的写成对象形式，就是所有的组件实例共用了一个data，这样改一个全都改了。

**Vue常用的修饰符**
答：.stop：等同于JavaScript中的event.stopPropagation()，防止事件冒泡；
.prevent：等同于JavaScript中的event.preventDefault()，防止执行预设的行为（如果事件可取消，则取消该事件，而不停止事件的进一步传播）；
.capture：与事件冒泡的方向相反，事件捕获由外到内；
.self：只会触发自己范围内的事件，不包含子元素；
.once：只会触发一次。

**.delete和Vue.delete删除数组的区别**
答：delete只是被删除的元素变成了 empty/undefined 其他的元素的键值还是不变。Vue.delete 直接删除了数组 改变了数组的键值。

**Vue slot**
答：简单来说，假如父组件需要在子组件内放一些DOM，那么这些DOM是显示、不显示、在哪个地方显示、如何显示，就是slot分发负责的活。

<a name="指令"></a>
## 指令
*v-model*
v-model 的使用。
答：v-model 用于表单数据的双向绑定，其实它就是一个语法糖，这个背后就做了两个操作：v-bind 绑定一个 value 属性；v-on 指令给当前元素绑定 input 事件。

Vue 中双向数据绑定是如何实现的？
答：Vue 双向数据绑定是通过 数据劫持 结合 发布订阅模式的方式来实现的， 也就是说数据和视图同步，数据发生变化，视图跟着变化，视图变化，数据也随之发生改变；
核心：关于 VUE 双向数据绑定，其核心是 Object.defineProperty() 方法。

说出几种 Vue 当中的指令和它的用法？
答：v-model 双向数据绑定；v-for循环；v-if v-show 显示与隐藏；v-on事件；v-once: 只绑定一次。

*v-事件*
v-on 可以监听多个方法吗？
答：可以，栗子：<input type="text" v-on="{ input:onInput,focus:onFocus,blur:onBlur, }">。

v-if 和 v-for 的优先级
答：当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的优先级，这意味着 v-if 将分别重复运行于每个 v-for 循环中。所以，不推荐 v-if 和 v-for 同时使用。
如果 v-if 和 v-for 一起用的话，Vue 中的的会自动提示 v-if 应该放到外层去。

**v-show 和 v-if 指令的共同点和不同点？**

<a name="Vue-cli"></a>
# Vue-cli
请说出 Vue-cli 项目中 src 目录每个文件夹和文件的用法？
答：assets 文件夹是放静态资源；components 是放组件；router 是定义路由相关的配置；app.Vue 是一个应用主组件；main.js 是入口文件。

assets 和 static 的区别
答：相同点：assets和static两个都是存放静态资源文件。项目中所需要的资源文件图片，字体图标，样式文件等都可以放在这两个文件下，这是相同点不相同点：assets中存放的静态资源文件在项目打包时，也就是运行npm run build时会将assets中放置的静态资源文件进行打包上传，所谓打包简单点可以理解为压缩体积，代码格式化。而压缩后的静态资源文件最终也都会放置在static文件中跟着index.html一同上传至服务器。static中放置的静态资源文件就不会要走打包压缩格式化等流程，而是直接进入打包好的目录，直接上传至服务器。因为避免了压缩直接进行上传，在打包时会提高一定的效率，但是static中的资源文件由于没有进行压缩等操作，所以文件的体积也就相对于assets中打包后的文件提交较大点。在服务器中就会占据更大的空间。建议：将项目中template需要的样式文件js文件等都可以放置在assets中，走打包这一流程。减少体积。而项目中引入的第三方的资源文件如iconfoont.css等文件可以放置在static中，因为这些引入的第三方文件已经经过处理，我们不再需要处理，直接上传。

**8、Vue-loader 是什么？使用它的用途有哪些？**
答：Vue 文件的一个加载器，将 template/js/style 转换成 js 模块。用途：js可以写es6、style样式可以scss或less、template可以加jade等


31.你们Vue项目是打包了一个js文件，一个css文件，还是有多个文件？
答：根据 Vue-cli 脚手架规范，一个js文件，一个CSS文件。

**axios 及安装?**
答：请求后台资源的模块。npm install axios --save 装好，js 中使用 import 进来，然后 .get 或 .post。返回在 .then 函数中如果成功，失败则是在 .catch 函数中。

35.axios 的特点有哪些
答：从浏览器中创建 XMLHttpRequests；
node.js 创建 http 请求；
支持Promise API；
拦截请求和响应；转换请求数据和响应数据；
取消请求；自动换成json。
axios中的发送字段的参数是data跟params两个，两者的区别在于params是跟请求地址一起发送的，data的作为一个请求体进行发送params一般适用于get请求，data一般适用于post put 请求。

<a name="生命周期面试题"></a>

# 生命周期面试题

**生命周期函数面试题**
什么是 Vue 生命周期？有什么作用？
答：每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做 生命周期钩子 的函数，这给了用户在不同阶段添加自己代码的机会。（ps：生命周期钩子就是生命周期函数）例如，如果要通过某些插件操作 DOM 节点，如想在页面渲染完后弹出广告窗， 那我们最早可在 mounted 中进行。

第一次页面加载会触发哪几个钩子？
答：beforeCreated， created， beforeMount， mounted

简述每个周期具体适合哪些场景
答：beforeCreate：在 new 一个 Vue 实例后，只有一些默认的生命周期钩子和默认事件，其他的东西都还没创建。在 beforeCreate 生命周期执行的时候，data 和 methods 中的数据都还没有初始化。不能在这个阶段使用 data 中的数据和 methods 中的方法。

created：data 和 methods 都已经被初始化好了，如果要调用 methods 中的方法，或者操作 data 中的数据，最早可以在这个阶段中操作。

beforeMount：执行到这个钩子的时候，在内存中已经编译好了模板了，但是还没有挂载到页面中，此时，页面还是旧的。

mounted：执行到这个钩子的时候，就表示 Vue 实例已经初始化完成了。此时组件脱离了创建阶段，进入到了运行阶段。 如果我们想要通过插件操作页面上的 DOM 节点，最早可以在和这个阶段中进行。

beforeUpdate： 当执行这个钩子时，页面中显示的数据还是旧的，data 中的数据是更新后的， 页面还没有和最新的数据保持同步。

updated：页面显示的数据和 data 中的数据已经保持同步了，都是最新的。

beforeDestory：Vue 实例从运行阶段进入到了销毁阶段，这个时候上所有的 data 和 methods ， 指令， 过滤器 ……都是处于可用状态。还没有真正被销毁。

destroyed： 这个时候上所有的 data 和 methods ， 指令， 过滤器 ……都是处于不可用状态。组件已经被销毁了。

created 和 mounted 的区别
答：created：在模板渲染成 html 前调用，即通常初始化某些属性值，然后再渲染成视图。
mounted：在模板渲染成 html 后调用，通常是初始化页面完成后，再对 html 的 DOM 节点进行一些需要的操作。

Vue 获取数据在哪个周期函数
答：一般 created/beforeMount/mounted 皆可.比如如果你要操作 DOM , 那肯定 mounted 时候才能操作.

请详细说下你对 Vue 生命周期的理解？
答：总共分为 8 个阶段创建前/后，载入前/后，更新前/后，销毁前/后。
创建前/后： 在 beforeCreate 阶段，Vue 实例的挂载元素 $el 和 **数据对象** data 都为 undefined，还未初始化。在 created 阶段，Vue 实例的数据对象 data 有了，$el 还没有。

载入前/后：在 beforeMount 阶段，Vue 实例的 $el 和 data 都初始化了，但还是挂载之前为虚拟的DOM 节点，data.message 还未替换。在mounted阶段，Vue 实例挂载完成，data.message 成功渲染。

更新前/后：当 data 变化时，会触发 beforeUpdate 和 updated 方法。

销毁前/后：在执行 destroy 方法后，对 data 的改变不会再触发周期函数，说明此时 Vue 实例已经解除了事件监听以及和 DOM 的绑定，但是 DOM 结构依然存在。