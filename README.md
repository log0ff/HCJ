筛选规则
年龄限制：一般限制在28岁以下，技术要求特别高的或者技术特别好的可以适当放宽
有电商开发项目经验的优先
有完整项目开发经验的优先
有技术深度项目开发经验的优先
技术栈吻合的优先
本科优先
会小程序的优先
技术栈关键词
ajax、html、css、jquery代表是个前端
Vue、React、Angular代表有现代化前端框架使用经验
Egg.js、Express.js代表有过node使用经验
webpack、gulp代表有过打包构建方面的优化经验
dom渲染、mvvm结构代表了解现代化前端框架架构
Vue系列（vue+vuex+vue-router+axios）
React系列（react+redux+dva+react-router+axios）
ES6、TypeScript、umi代表关注新的前端发展
npm、yarn包管理工具
less、sass前端样式
Vant、Ant Design、Element  UI框架
electron桌面客户端封装
sqllite桌面数据库
项目经验判断
简历不同类型项目的多少，这点主要看覆盖面，比如电商类的项目做过，教育类的项目也做过，就是覆盖了两个不同类型的业务方向
如果简历中大部分是管理端，则业务逻辑需要重点判断，如果是客户端，则可以询问浏览器兼容、自适应问题
询问是否完整经历过一个项目的开发
常用面试问题
1.Vue的生命周期及应用
1）生命周期:beforeCreate、created、beforeMount、mounted、beforeUpdate、updated、beforeDestroy、destroyed
能说出created、mounted、updated、destroyed即可

breforeCreate（）：实例创建前，这个阶段实例的data和methods是读不到的。

created（）：实例创建后，这个阶段已经完成数据观测，属性和方法的运算，watch/event事件回调，mount挂载阶段还没有开始。$el属性目前不可见，数据并没有在DOM元素上进行渲染。

created完成之后，进行template编译等操作，将template编译为render函数，有了render函数后才会执行beforeMount（）

beforeMount（）：在挂载开始之前被调用：相关的 render 函数首次被调用

mounted（）：挂载之后调用，el选项的DOM节点被新创建的 vm.$el 替换，并挂载到实例上去之后调用此生命周期函数，此时实例的数据在DOM节点上进行渲染

后续的钩子函数执行的过程都是需要外部的触发才会执行

有数据的变化，会调用beforeUpdate，然后经过Virtual Dom，最后updated更新完毕，当组件被销毁的时候，会调用beforeDestory，以及destoryed。

2）应用：页面中定义一个定时器，在哪个阶段清除？
答案：在 beforeDestroy 中销毁定时器。

①为什么销毁它：

在页面a中写了一个定时器，比如每隔一秒钟打印一次1，当我点击按钮进入页面b的时候，会发现定时器依然在执行，这是非常消耗性能的。

②解决方案1：

        mounted(){
            this.timer = setInterval(()=>{
            console.log(1)
        },1000)
        },
        beforeDestroy(){
            clearInterval(this.timer)
        }
方案2（推荐）：该方法是通过$once这个事件侦听器在定义完定时器之后的位置来清除定时器

        mounted(){
             const timer = setInterval(()=>{
                console.log(1)
             },1000)
             this.$once('hook:beforeDestroy',()=>{
                clearInterval(timer)
             })
        }
答出1即可，答出2就是厉害。
推荐方案2的原因：
它需要在这个组件实例中保存这个 timer，如果可以的话最好只有生命周期钩子可以访问到它。这并不算严重的问题，但是它可以被视为杂物。我们的建立代码独立于我们的清理代码，这使得我们比较难于程序化的清理我们建立的所有东西。

2.Vue子父组件传值
父组件向子组件传值

子传父 $emit

能回答出$emit即可。

<!--父组件-->
    Vue.component('parent', {
        template: `
            <div>
                <child @show-txt="show"></child>
                <div v-if="name"> name: {{ name }} </div>
                <div v-if="age"> age: {{ age }}</div>
            </div>
        `,
        data() {
            return {
                name: '',
                age: ''
            }
        },
        methods: {
            show(data) {
                this.name = data.name;
                this.age = data.age;
            }
        }
    });
    <!--子组件-->
    Vue.component('child', {
        template: `        <button @click="on_click">btn</button>
        `,
        methods: {
            on_click() {
                this.$emit('show-txt', {name: 'Rabbit', age: 18})
            }
        }
    });
父传子 props

能回答出props即可。

    <!--父组件-->
    <template id="parent">
        <div>
            <child :message="msg"></child>
        </div>
    </template>
<!--子组件-->
    <template id="child">
        <div>
            {{message}} 
        </div>
    </template>
    /*子组件*/
    Vue.component('child', {
        template: '#child',
        props: ['message']
    })
    /*建立Vue实例*/
    new Vue({
        el: '#app',
        data: {
            msg: 'Hello'
        },
        /*父组件*/
        components: {
            'parent': {
                template: '#parent',
                data() {
                    return {
                        msg: 'Hello'
                    }
                }
            }
        }
    })
3.ES6语法 Promise封装axios
这个简单主要是考察一步promise，包括resolve和reject，以下是最简单的案例，可以继续考察如何传入一个全局Token及如何判断鉴权失败等。

    export function get (url,parms){
        return Promise((resolve,reject)=>{
            axios.get(url,{
                params,params
            }).then(res=>{
                resolve(res.data);
            }).catch(err=>{
                reject(err.data)
            })
        })
    }
4.watch 和 computed的区别？
computed：
①有缓存机制；②不能接受参数；③可以依赖其他computed，甚至是其他组件的data；④不能与data中的属性重复
watch：
①可接受两个参数；②监听时可触发一个回调，并做一些事情；③监听的属性必须是存在的；④允许异步
watch配置：handler、deep（是否深度）、immeditate （是否立即执行）
总结：
当有一些数据需要随着另外一些数据变化时，建议使用computed
当有一个通用的响应数据变化的时候，要执行一些业务逻辑或异步操作的时候建议使用watch
5.Vue-Router导航钩子函数有哪些？页面如何带参数跳转？
前置守卫（这边可以问下前置守卫可以用来干嘛，答：可以生成动态路由实现权限功能）

    router.beforeEach((to, from, next) => {
      // do someting
    });
后置钩子（没有next参数）

    router.afterEach((to, from) => {
      // do someting
    });
跳转:分为以下两种：

params
传参
    this.$router.push({
     name:"detail",
     params:{
       name:'xiaoming',
     }
    });
接受
this.$route.params.name
query
传参
this.$router.push({
  path:'/detail',
  query:{
    name:"xiaoming"
  }
 })
接受 //接收参数是this.$route
this.$route.query.id
6. ES6的特有的类型， 常用的操作数组的方法都有哪些？
es6新增的主要的特性：

① let const 两者都有块级作用域

② 箭头函数

③ 模板字符串

④ 解构赋值

⑤ for of循环

⑥ import 、export 导入导出

⑦ set数据结构

⑧ …展开运算符

⑨ 修饰器 @

⑩ class类继承

⑪ async、await

⑫ promise

⑬ Symbol

⑭ Proxy代理

操作数组常用的方法：

es5：concat 、join 、push、pop、shift、unshift、slice、splice、substring和substr 、sort、 reverse、indexOf和lastIndexOf 、every、some、filter、map、forEach、reduce

es6：find、findIndex、fill、copyWithin、Array.from、Array.of、entries、values、key、includes

7.vue双向绑定原理？
通过Object.defineProperty()来劫持各个属性的setter,getter，在数据变动时发布消息给订阅者，触发相应的监听回调
8.如何解决跨域
后端：配置header，使用option预检请求
前端：使用proxy代理
9.$nextTick用过吗，有什么作用？
在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

解决的问题：有些时候在改变数据后立即要对dom进行操作，此时获取到的dom仍是获取到的是数据刷新前的dom，无法满足需要，这个时候就用到了$nextTick。

10.Vuex的五种属性？dispatch与commit区别？
1、state：vuex的基本数据，用来存储变量(后四个属性都是用来操作state里面储存的变量的)。

2、getters：是对state里面的变量进行过滤的。

3、mutation:提交更新数据的方法，必须是同步的(如果需要异步使用action)。

4、action：和mutation的功能大致相同，不同之处在于：

1）Action提交的是mutation，而不是直接变更状态。 也就是action是用来修改mutation并提交的 而 mutation是通过修改state

2）Action可以包含任意异步操作。(一般比较复杂的数据都在action中操作)

3）action先会执行异步操作再去调用mutation，随后才跟新state

5、modules：项目特别复杂的时候，可以让每一个模块拥有自己的state、mutation、action、getters,使得结构非常清晰，方便管理。

dispatch：是跟action一块用的，含有异步操作，例如向后台提交数据，
写法：    this.$store.dispatch('mutation的方法名',获取值)
commit：是跟mutation一块用的，同步操作 ，
写法： this.$store.commit('mutation的方法名',获取值)
技术深度判断
webpack打包优化
TypeScript是否使用过
是否了解Vue的底层原理（比如diff算法）
是否参与过开源项目
dll的调用与封装
浏览器与桌面客户端的通信解决方案与流程
