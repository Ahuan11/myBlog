#  Vue2基础知识

## **监视属性Watch**

**watch：**

1.当被监视的属性发生变化时，回调函数自动调动，进行相关操作

2.监视的属性必须存在，才能进行监视

3.深度监视：

​			（1） Vue中的watch默认不监测对象内部值的改变（一层）。

​			（2） 配置deep：true可以检测对象内部值的改变（多层）。

```javascript
watch:{
	xxx():{
	//handler什么时候调用，当xxx属性发生改变时调用
        handler(newValue,oldValue){
			console.log(newValue,oldValue)
        }
	}

-----------------------------------------------------------------
watch:{
    //监视多级结构中某个属性的变化
    'aaa.bbbb':{
        handler(){
            
        }
    }
}
-------------------------------------------------------------------------watch:{
    aaa:{
        deep:true,//深度监听
        immediate：true,//初始化时让handler调用一下
        handler(){

       }
    }
}
    
  
```

###### **computed 和watch之间的区别：**

​		1.computed能完成的功能，watch都可以完成。

​		2.watch能完成的功能，computed不一定能完成，例如：watch可以进行异步操作

两个重要的小原则：

​		1.所被Vue管理的函数，最好写成普通函数，这样this的指向才是 vm 或 组件实例对象

​		2.所有不被Vue管理的函数（定时器的回调函数，ajax的回调函数等），最好写成箭头函数，

​			这样this的指向才是vm（view model） 或 组件实例对象



## **样式绑定（v-bind）**

###### 	**1.class样式**

​				写法：class="xxx"    xxx可以是**字符串**，**对象**，**数组**。

​							字符串写法适用于：类名不确定，要动态获取。

​							对象写法适用于：要绑定多个样式，个数不确定，名字也不确定

​							数组写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用。

###### 	2.style样式

```javascript
:style="{fontSize: xxx}"   //其中xxx是动态值
:style="[a,b]"             //其中a，b是样式对象
```



## 条件渲染：

###### 	1.v-if    写法：

​						（1） v-if="表达式"

​						（2）v-else-if="表达式"

​						（3）v-else="表达式"

​				适用于：切换频率较低的场景。

​				特点：不展示的DOM元素直接移除。

​				注意：v-if  可以和  v-else-if、v-else一起使用，但要求结构不能被"打断"

###### 	2.v-show  写法：

​					（1）v-show="表达式"

​							适用于：切换频率较高的场景。				

​							特点：不展示的DOM元素未被移除，仅仅是使用样式隐藏掉

###### 3.v-model  

​						（1）双向绑定

```html
//在vue2中可以用以下方实现v-model的功能
//v-model原理是通过value和input实现的
<input type='text' :value="msg" @input="msg = $event.target.value "/>
```

###### 4.sync属性修饰符【组件通信的一种】

​							可以实现父子组件数据同步

​	:money.sync，代表父组件给字符串传递props【money】给当前子组件绑定一个自定义事件【update:money】



###### 5.$attrs

​				（1）属于组件的一个属性，可以获取到父组件传递过来的props数据

​				（2）对于子组件而言，父组件给的数据可以利用props接收，但是需要注意，如果子组件通过props接受的属性，在$attrs属性当中是获取不到的

###### 6.$listeners

​				（1）$listeners，它是组件实例自身的一个属性，他可以获取到父组件给子组件传递的自定义事件

```html
<el-button v-bind="$attrs" v-on="$listeners"></el-button>
//不能用：和@ 代替
```

###### 7.$children

​					（1）是组件实例的属性，可以获取到当前组件的全部子组件

###### 8.$parent 

​					（1）是组件实例的属性，可以获取到当前组件的父组件，进而可以操作父组件的数据和方法

## 面试题：react、vue中的key有什么作用？（key的内部原理）

1.虚拟DOM中key的作用：

​		key是虚拟DOM对象的标识，当数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOM】，

​		随后Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较，比较规则如下：

2.对比规则：

​	（1）旧虚拟DOM中找到了与新虚拟DOM相同的key：	

​					a.  若虚拟DOM中内容没变，直接使用之前的真实DOM！

​					b. 若虚拟DOM中内容变了，则生成新的真实DOM，随后替换掉页面之前的真实DOM。

​	（2）旧虚拟DOM中未找到与新虚拟DOM相同的key

​						创建新的真实DOM，随后渲染到页面。

3.用index作为key可能会引发的问题：

​			（1）若对数据进行：逆序添加，逆序删除等破坏顺序操作：

​								会产生没有必要的真实DOM更新 ==》 界面效果没有问题，但效率低。

​			（2） 若结构中还包含输入类的DOM：

​								会产生错误DOM更新 ==》 界面有问题。

4.开发中如何选择key？：

​				1.最好使用每条数据的唯一标识作为key，比如id，手机号、身份证号、学号等唯一值。

​				2.如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示。

​					使用index作为key是没有问题的



Vue监视数据的原理：

​	1.vue会监视data中的所有层次的数据

​	2.如何监测对象中的数据?

​			通过setter实现监视，且要在new Vue时就传入要监测的数据。

​					（1）对象中后追加的属性，Vue默认不做响应式处理

​					（2）如需给后添加的属性作响应式，请使用如下API：

```
			Vue.set(target,propertyName/index,value) 或
			vm.$set(target,propertyNAme/index,value)
```

​	3.如何监测数组中的数据？

​					通过包裹数组更新元素的方法实现，本质就是做了两件事：

​						（1）调用原生对应的方法对数组进行更新。

​						   (2)  重新解析模板，进而更新页面

4. 在Vue修改数组中的某个元素一定要用如下方法：

   ​		（1）使用这些API   :   push()、 pop()、shift()  、unshift()  、splice() 、sort()  、reverse()

   ​		（2）Vue.set()  或   vm.$set()

   !!!!!!!注意：Vue.set()   和    vm.$set()  不能给 vm 或 vm 的根数据对象添加属性



## 收集表单数据：

​			若：<input type="text"/>  ,  则v-model收集的是value值，用户输入的就是value值。

​			若： <input type="radio"/> ，则v-model收集的是value值，且要给标签配置value值

​			若：<input type="checkbox"/>

​						1.没有配置input的value属性，那么收集的就是checked （勾选 or未勾选 是布尔值）

​						2.配置input的value属性：

​								（1）v-model的初始值是非数组，那么收集的就是checked（勾选 or未勾选，是											布尔值）

​								（2）x-model的初始值是数组，那么收集的就是value组成的数组

​			备注：v-model的三个修饰符：

​								lazy：失去焦点再收集数据

​								number：输入字符串转为有效的数字

​								trim：输入首尾空格过滤



## 过滤器：

​	定义：对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）

​	语法：

​				1.注册过滤器：Vue.filter ( name , callback )    -----全局过滤器   或

​											 new Vue(filter:{}) --------局部过滤器

​				2.使用过滤器：{{  xxx  |  过滤器名  }}  或   v-bind:属性 = " xxx |  过滤器"

​	备注：

​				1.过滤器也可以接收额外参数，多个过滤器也可以串联

​				2.并没有改变原本的数据，是产生新的对应的数据



Vue基本指令

```
v-bind  ：单向绑定解析表达式，可简写为  :xxx
v-model ：双向数据绑定
v-for   ：遍历数组/对象/字符串
v-on	：绑定事件监听，可简写为@
v-if    ：条件渲染（动态控制节点是否存在）
v-else  ：条件渲染（动态控制节点是否存在）
v-show  ：条件渲染（动态控制节点是否展示）

v-text指令：
		1.作用：向其所在的节点中渲染文本内容。
		2.与插值语法的区别：v-text会替换掉节点中的内容，{{xxx}}不会

v-html指令：
	1.作用：向指定节点中渲染包含HTML结构的内容
	2.与插值语法的区别：
	（1）v-html会替换掉节点中所有的内容，{{xxx}}不会
	（2）v-html可以识别html结构
	3.严重注意：v-html有安全性问题！！
	（1）在网站上动态渲染任意HTMl是非常危险的，容易导致xss攻击
	（2）一定要在可信的内容上使用v-html，永不要用在用户提交的内容上
	
v-cloak指令（没有值）：
	1.本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性。
	2.使用css配合v-cloak可以解决网速慢时页面展示出{{xxx}}的问题

v-once指令：
	1.v-once所在节点在初次动态渲染后，就视为静态内容
	2.所有数据的改变不会引起v-once所在结构的更新，可以用于优化性能

v-pre指令：
	1.vue会跳过其所在节点的编译过程
	2.可利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译
```



## vue自定义指令：

自定义指令总结：

​	1.定义语法：

​			（1）局部指令：

```
new Vue({
	directives:{指令名：配置对象}
})
或
new Vue({
	directives{指令名:回调函数}
})
```

​			（2）全局指令：

```
Vue.directive(指令名，配置对象)
或
Vue。directive(指令吗，回调函数)
```

​	2.配置对象中常用的三个回调：

​		![1651382091240](E:\学习笔记\钩子函数)

​      3 .备注：

​				1.指令定义时不加v-，但使用时要加v-；

​				2.指令名如果是多个单词，要使用  “-”进行连接，不要用驼峰命名法。



## 生命周期：

​	1.又名：生命周期回调函数、生命周期函数、生命周期钩子

​	2.是什么：Vue在关键时刻帮我们调用的一些特殊名称的函数

​	3.生命周期函数的名字不可更改，但函数的具体内容是程序员根据需求编写的。

​	4.生命周期函数中的this指向是vm或组件实例对象

常用的生命周期钩子：

​	1.mounted：发送ajax请求、启动定时器、绑定自定义事件、订阅消息等【初始化操作】

​	2.beforeDestroy：清除定时器、解绑自定义事件、取消订阅消息等【收尾工作】

关于销毁Vue实例：

​	1.销毁后借助Vue开发者工具看不到任何消息

​	2.销毁后自定义事件会失效，但原生DOM事件依然有效

​	3.一般不会在beforeDestroy操作数据，因为即使操作数据，也不会再出发更新流程了。



## ref属性：

​		1.被用来给元素或子组件注册引用信息（id的替代者）

​		2.应用在html标签上获取的是真实DOM元素，应用在组件标签上的是组件实例对象（vc)

​		3.使用方式：	

```html
		打标识：<h1 ref='xxx'>.....</h1>或<School ref="xxx"></School>
		获取：this.$ref.xxx
```

## **props配置：**

功能：让组件接收外部传过来的数据

```
(1).传递数据：
		<Demo name="xxx"/>
(2).接收数据：
		第一种方式（只接收）：
			props:['name']
		第二种方式（限制类型）：
			props:{
				name:Number
			}
		第三种方式（限制类型、限制必要性、指定默认值）：
			props:{
				name:{
					type:String,//类型
					required:true,//必要性
					default:'老王' //如果不传，则给默认值'老王'，default不与required一起用
					
				}
			}
```

**！！！注意：**

​	**props是只读的，**Vue底层会监测你对props的修改，如果进行了修改，就会发出警告，若业务需求确实需要修改，那么请复制props的内容到date中一份，然后去修改data中的数据



## mixin(混入)

```
功能：可以把多个组件共用的配置提取成一个混入对象
使用方式：
	第一步定义混合：例如：
		{
			data(){...},
			methods:{...}
			....
		}
	第二步使用混入，例如：
		 (1)全局混入：Vue.mixin(xxx)
		 (2)局部混入：mixins:['xxx']
```



## 插件

功能：用于增强Vue

本质：包含install方法的一个对象，install的第一个参数是Vue，第二个以后的参数是插件使用者传递的数据。

定义插件：

```=
对象.install = function(Vue,options){
	//1.添加全局过滤器
	Vue.filetr(...)
	
	//2.添加全局指令
	Vue.directive(...)
	
	//3.配置全局混入（合）
	Vue.mixin(...)
	 
	 //4.添加实例方法
	 Vue.prototype.$myMethod = function(){...}
	 Vue.prototype.$myProperty = xxx
}

使用插件：Vue.use()
```

## 总结TodoList案例

###### 1.组件化编码流程

​	（1）拆分静态组件：组件要按照功能点拆分，命名不要与html元素冲突。

​	（2）实现动态组件：考虑好数据的存放位置，数据是一个组件在用，还是一些组件在用：

​					a.  一个组件在用：放在组件自身即可。

​					b. 一些组件在用：放在他们共同的组件上（状态提升）。

​	（3）实现交互：从绑定事件开始。

###### 2.props适用于：

​	（1）父组件 ==> 子组件 通信

​	（2）子组件 ==> 父组件 通信（要求父组件先给子组件一个函数）

3.使用v-model时要切记：v-model绑定的值不能是props传过来的值，因为props是不可以修改的！

4.props传过来的若是对象类型的值，修改对象中的属性时Vue不会报错，但不推荐这样做。



## WebStorage（SessionStorage 和 LocalStorage的统称）

​		1.存储内容大小一般支持5MB左右（不同浏览器可能还不一样）

​		2.浏览器端通过  Window.sessionStorage   和   Window.localStorage  属性来实现本地存储机制

​		3.相关API：

```javascript
	1.xxxxxStorage.setItem('key','value');
 	//该方法接受一个键和值作为参数，会把键值对添加到存储中，如果键名存在，则更新其对应的值
	2.xxxxxStorage.getItem('person');
	//该方法接受一个键名作为参数，返回键名对应的值
	3.xxxxxStorage.removeItem('key');
	//该方法接受一个键名作为参数，并把该键名从存储中删除
	4.xxxxxStorage.clear();
	//该方法会清空存储中的所有数据
```

​		4.备注：

```
	1.  SessionStorage  存储的内容会随着浏览器窗口关闭而消失
	2.  LocalStorage    存储的内容，需要手动清除才会消失
	3.  xxxxxStorage.getItem(xxxx)  如果xxxx对应的value获取不到，你那么getItme的返回值是null
	4.  JSON.parse(null)的结果依然是 null（作用：将JSON字符串转换成对象）
	5.  JSON.stringify():将js对象转换为JSON格式
```

## 组件的自定义事件

​	1.一种组件间通信的方式，适用于：子组件 ==> 父组件

​	2.使用场景：A是父组件，B是子组件，B想给A传数据，那么就要在A中给B绑定自定义事件（事件回调在A中）

​	3.绑定自定义事件：

​			(1).第一种方式， 在父组件中：<Demo @atguigu=“test”/> 或 <Demo v-on:atguigu="test"/>

​			(2).第二种方式，在父组件中：

```
	<Demo ref="demo"/>
	......
	mounted(){
		this.$refs.xxx.$on('atguigu',this.test)
	}
```

​			(3).若想要自定义事件只能触发一次，可以使用  **once**  修饰符，或  **$once** 方法

​	4.触发自定义事件：

```
		this.$emit('atguigu',数据)
```

​	5.解绑自定义事件

```
		this.$off('atguigu')
		this.$off(['atguigu','demo'])
		this.$off()      //解绑所有自定义事件
```

​	6.组件上也可以绑定原生DOM事件，需要使用native修饰符

​	7.注意：通过  this.$refs.xxx.$on(‘atguigu’,回调)   绑定自定义事件时，**回调要么配置在methods中，要么用箭头函数，否则this指向会出问题**！



## 全局事件总线（GlobaEventBus）

1.一种组件间通信的方式，适用与任意组件间通信。

2.安装全局事件总线：

```
new Vue({
	....
	beforeCreate(){
		Vue.prototype.$bus = this
	},
})
```

3.使用事件总线：

​		（1）接收数据：A组件想接收数据，则在A组件中给$bus绑定自定义事件，事件的回调留在A组件自身

```
methods(){
	demo(data){....}
}
....
mounted(){
	this.$bus.$on('xxxx',this.demo)
}
```

​		（2）提供数据：

```
		this.$bus.$emit('xxx',数据)
```

4.最好在beforeDestroy钩子中，用$off去解绑**当前组件所用到的**事件



## 消息订阅与发布（pubsub）

1.一种组件间通信的方式，适用于任意组件间通信

2.使用步骤：

```
	(1)安装pubsub：npm i pubsub-js
	(2)引入：import pubsub from 'pubsub-js'
	(3)接收数据：A组件想接收数据，则在A组件中订阅消息，订阅的回调留在A组件自身
		methods(){
			demo(data){....}
		}
		.....
		mounted(){
			this.pid = pubsub.subscribe('xxx',this.demo)//订阅消息
		}
	(4)提供数据：pubsub.publish('xxx',数据)
	(5)最好在beforeDestroy钩子中，用pubsub.unsubscribe(pid)去取消订阅
```

## nextTick

1.语法：this.$nextTick(回调函数)

2.作用：在下一次DOM更新结束后执行其指定的回调

3.什么时候用：当改变数后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行



## Vue封装的过渡与动画

1.作用：在插入、更新或移除DOM元素时，在合适的时候给元素添加样式类名

2.写法：

​		（1）准备好样式：

​						元素进入的样式：

​								  a.   v-enter :  进入的起点

​								  b.  v-enter-active：进入过程中

​								  c.  v-enter-to：进入的终点

​						元素离开的样式：

​							     a.   v-leave :  离开的起点

​								  b.  v-leave-active：离开过程中

​								  c.  v-leave-to：离开的的终点

​		（2）使用<transition>包裹要过度的元素，并配置name属性：

```html
<transition name='hello'>
    <h1 v-show="isShow">你好啊！</h1>
</transition>
```

​		（3）备注：若有多个元素需要过渡，则需要使用：<transition-group>，且每个元素都要指定key值



## Vue脚手架配置代理

#### 方法一

​	在vue.config.js中添加如下配置：

```
devServe:{
	proxy:"http://localhost:5000"
}
```

说明：

​		1.优点：配置简单，请求资源时直接发给前端（8080）即可

​		2.缺点：不能配置多个代理，不能灵活的控制请求是否走代理

​		3.工作方式：若按照上述配置代理，当请求了前端不存在的资源时，那么该请求会转发给服务器（优先匹配前端资源）

#### 方法二

​	编写vue.config.js配置具体代理规则：

```javascript
module.exports = {
	devServer:{
		proxy:{
		'/api1':{  //匹配所有以'/api1'开头的请求路径
			target:'http://localhost:5000',//代理目标的基础路径
            changeOrigin:true,
            pathRewrite:{'^/api1':''}//重写路径
			},
        '/api2':{   //匹配所有以'/api2'开头的请求路径
            target:'http://localhost:5001',//代理目标的基础路径
            changeOrigin:true,
            pathRewrite:{'^/api2':''}//重写路径
         	}
		}
	}
}
/*
	changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
	changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:8080
	Vue中  changeOrigin默认值为true
	react中  changeOrigin默认值为false
*/
```

说明：

​		1.优点：可以配置多个代理，且可以灵活的控制请求是否走代理

​		2.缺点：配置略微繁琐，请求资源必须加前缀

#### 

## 插槽

​		1.作用：让父组件可以向子组件指定位置插入html结构，也是一种组件间通信的方式，适用于  **父组件  === >  子组件**

​		2.分类：默认插槽、具名插槽、作用域插槽

​		3.使用方式：

### 					(1)默认插槽：

```html
		父组件中：
			<Category>
                <div>html结构1</div>
			</Category>
		子组件中：
			<template>
                <div>
                    <!--定义插槽-->
                    <slot>插槽默认内容</slot>
                </div>
			</template>
			
```

### 					(2)具名插槽：

```html
		父组件中：
			<Category>
				<template slot="center">
                     <div>html结构1</div>
                </template>
				<template v-slot:footer>
                     <div>html结构2</div>
                </template>
			</Category>
		子组件中：
			<template> 
                <div>
                    <!--定义插槽-->
                    <slot name="center">插槽默认内容</slot>
                    <slot name="footer">插槽默认内容</slot>
                </div>
			</template>
```

### 					(3)作用域插槽：

​								a.   理解：数据在组件的自身，但根据数据生成的结构需要组件的使用者来决定，（games数据在Category组件中，但使用数据所遍历出来的结构由App组件来决定）

​								b.   具体编码：

```html
父组件中：
	<Category>
        <template scope="scopeData">  
            <!--生成的是ul列表-->
            <ul>
                <li v-for=" g in scopeData.games" :key="g">{{g}}</li>
            </ul>
        </template>
	</Category>

	<Category>
        <template slot-scope="scopeData">  
            <!--生成的是h4标题-->
          	<h4 v-for="g in scopeData.games" :key="g">
                {{g}}
            </h4>
        </template>
	</Category>
子组件中：
	<template>
        <div>
            <slot :games="games"></slot>
        </div>
	</template>
	<script>
        export default{
            name:"Category",
            props:['title'],
            data(){
                return{
                    games:['红色警戒','穿越火线','劲舞团','超级玛丽']
                }
            }
        }
	</script>
```



## Vuex

### 1.概念	

​	在Vue中实现集中式状态（数据）管理的一个Vue插件，对 vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件之间通信

### 2.何时使用？

​	多个组件需要共享数据时

### 3.搭建Vuex环境

​		1.创建文件：src/store/index.js

```javascript
//引入Vue核心库
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//应用Vuex插件
Vue.use(Vuex)

//准备actions对象 ---响应组件中用户的动作
const actions = {}
//准备mutations对象 ---修改state中的数据
const mutations = {}
//准备state对象----保存具体的数据
const state = {}

//创建并暴露store
export default new Vuex.Store({
    actions,
    mutations,
    state
})
```

​		2.在main.js中创建vm时传入store配置项

```javascript
....
//引入store
import store from './store'
....

//创建vm
new Vue({
	el:'#app',
	render:h=>h(App),
	store
})
```

### 4.基本使用

​			1.初始化数据、配置actions、配置mutations、操作文件store.js

```javascript
//引入Vue核心库
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//应用Vuex插件
Vue.use(Vuex)


const actions = {
    //响应组件中加的动作
    jia(context,value){
        //console.log('actions中的jia被调用了',context,value)
        context.commit('JIA',value)
    },
}

const mutations = {
    //响应组件中加的动作
    JIA(state,value){
        //console.log('mutations中的JIA被调用了',state,vale)
        state.sum+=value
    }
}
//初始化数据
const state = {
    sum=0
}

//创建并暴露store
export default new Vuex.Store({
    actions,
    mutations,
    state
})
```

​		2.组件中读取Vuex中的数据：$store.state.sum

​		3.组件中修改vuex中的数据：

```javascript
		$store.dispatch('actions中的方法名',数据)
		或
        $store.commit('mutations中的方法名',数据)
```

​			注意：若没有网络请求或其他业务逻辑，组件也可以越过actions，即不写dispatch，直接编写commit



## 四个map方法的使用

### 1.mapState方法：用于帮助我们映射state中的数据为计算属性

```javascript
computed:{
	//借助mapSate生成计算属性，sum 、school、subject（对象写法）
    ...mapState({sum:'sum',school:'school',subject:'subject'}),
    //借助mapState生成计算属性，sum 、school 、subject（数组写法）
    ...mapState(['sum','school','subject']),
}
```

### 2.mapGetters方法：用于帮助我们映射getters中的数据为计算属性

```javascript
computed:{
	//借助mapGetters生成计算属性：bigSum(对象写法)
    ...mapGetters({bigSum:'bigSum'}),
    //借助mapGetters生成计算属性：bigSum(数组写法)
     ...mapGetters(['bigSum']),
}

```

### 3.mapActions方法：用于帮助我们生成actions对话的方法，即：包含$store.dispatch(xxx)的函数

```javascript
methods:{
	//靠mapActions生成：incrementOdd、incrementWait（对象形式）
	...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
	//靠mapActions生成：jiaOdd、jiaWait（数组形式）
	...mapActions(['jiaOdd','jiaWait'])
}
```

### 4.mapMutations：用于帮助我们生成mutations对话的方法，即：包含$store.commit(xxx)的函数

```javascript
methods:{
	//靠mapMutations生成：increment、increment（对象形式）
	...mapMutations({increment:'JIA',increment:'JIAN'})
	//靠mapMutations生成：JIA、JIAN（数组形式）
	...mapMutations(['JIA','JIAN'])
}
```

> 备注：mapActions与mapMutations使用时，若需要传递参数：在模板中绑定事件时传递好参数，否则参数是事件对象
>



## 模块化+命名空间

​			1.目的：让代码更好维护 ，让更多数据分类更加明确

​			2.修改store.js

```javascript
const countAbout = {
	namespaced:true, //开启命名空间
	state:{x:1},
	mutations:{....},
	actions:{....},
	getters:{
		bigSum(state){
			return state.sum*10
		}
	}
}

const personAbout = {
	namespaced:true, //开启命名空间
	state:{...},
	mutations:{....},
	actions:{....},
}
    
const store = new Vuex.Store({
    modules:{
    countAbout,
    personAbout
	}
})

```

3.开启命名空间后，组件中读取state数据：

```javascript
//方式一：自己直接读取
this.$store.state.personAbout.list
//方式二：借助mapState读取
...mapState('countAbout',['sum','school','subject'])

```

4.开启命名空间后，组件中读取getters数据：

```javascript
//方式一：自己直接读取
this.$store.getters('personAbout/firstPersonName')
//方式二：借助mapState读取
...mapGetters('countAbout',['bigSum'])

```

5.开启命名空间后，组件中调用dispatch

```javascript
//方式一：自己直接dispatch
this.$store.dispatch('personAbout/addPersonWang',person)
//方式二：借助mapActions
...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'})

```

6.开启命名空间后，组件中调用commit

```javascript
//方式一：自己直接commit
this.$store.commit('personAbout/ADD_PERSON',person)
//方式二：借助mapMutations
...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'})

```



## 路由

​	1.理解：一个路由（route）就是一组映射关系（key-value），多个路由需要路由器（router）进行管理

​	2.前端路由：key是路径，value是组件

### 1.基本使用

​	1.安装vue-router，命令：npm i vue-router

​	2.应用插件：Vue.use(VueRouter)

​	3.编写router配置项：

```javascript
//引入VueRouter
import VueRouter from 'vue-router'
//引入路由组件
import About from '../components/About'
import Home from '../components/Home'
//创建router实例对象，去管理一组一组的路由规则
const router = new VueRouter({
    routes:[
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home
        },
    ]
})

export default router
```

​	4.实现切换（active-class可配置高亮样式）

```html
<router-link active-class="active" to="/about">About</router-link>
```

​	5.指定展示位置

```html
<router-view></router-view>
```

### 2.几个注意点

​	1.路由组件通常存放在pages文件夹，一般组件通常存放在components文件夹

​	2.通过切换，“隐藏”了路由组件，默认是被销毁掉的，需要的时候再去挂载

​	3.每个组件都有自己的$route属性，里面存储着自己的路由信息

​	4.整个应用只有一个router，可以通过组件的$router属性获取到

### 3.多级路由（嵌套路由）

​	1.配置路由规则，使用children配置项：

```javascript
routes:[
    {
        path:'/about',
        component:About,
    },
    {
        path:'/home',
        components:Home,
        children:[
            {
                path:'news',
                component:News
            },
            {
                path:'message',
                component:Message
            }
        ]
    }
]
```

​	2.跳转（要写完整路径）

```html
<router-link to="/home/news">News</router-link>
```

4.路由的query参数

​	1.传递参数

```html
<!--跳转并且带query参数，to的字符串写法-->
<router-link :to="`/home/message/detail?id=666&title=你好`">跳转</router-link>
<!--跳转并且带query参数，to的对象写法-->
<router-link
   :to="{
        path:'/home/message/detail',
        query:{
        	id:666,
        	title:'你好'
        }
    }"
>
    跳转
</router-link>
```

​	2.接收参数：

```javascript
$route.query.id
$route.query.title
```

### 5.命名路由

​	1.作用：可以简化路由的跳转。

​	2.如何使用

​			（1）给路由命名：

```javascript
{
	path:'/demo',
    component:Demo,
        children:[
            {
                path:'test',
                component:Test,
                children:[
                    name:'hello',
                    path:'welcome',
                    component:Hello,
                ]
            }
        ]
}
```

​			（2）简化跳转：

```html
<!--简化时，需要写完整的路径-->
<router-link to="/demo/test/welcome">跳转</router-link>

<!--简化后，直接通过名字跳转-->
<router-link :to="{name:'hello'}">跳转</router-link>

<!--简化写法配合传递参数-->
<router-link
      :to="{
          name:'hello',
          query:{
           	id:666,
           	title:'你好'
          }
      }"        
>跳转</router-link>
```



### 6.路由的params参数

​	1.配置路由，声明接收params参数

```javascript
{
	path:'/home',
    component:Home,
    children:[
        path:'news',
        component:News
    ],
    {
        component:Message,
        children:[
            {
                name:'xiangqing',
                path:'detail/:id/:title',//使用占位符声明接收params参数
                component:Detail
            }
        ]
    }
}
```

​	2.传递参数

```html
<!--跳转并携带params参数，to的字符串写法-->
<router-link :to="/homr/message/detail/666/你好">跳转</router-link>
<!--跳转并携带params参数，to的对象写法-->
<router-link 
     :to="{
          name:'xiangqing',
          params:{
              id:666,
              title:'你好'
     	  }
 	}"
             
>跳转</router-link>
```

​	特别注意：路由携带params参数时，若使用to的对象写法，则不能使用path配置项，必须使用name配置项！

​	3.接收参数：

```
$route.params.id
$route.params.title
```

### 7.路由props配置

​	作用：让路由组件更方便的收到参数

```javascript
{
	name:'xiangqing',
	path:'detail/:id',
	component:Detail,
	//第一种写法：props值为对象，该对象中所有的key-value的组合最终都会通过props传给Detail组件
    //props:{a:900}
    //第二种写法：props值为布尔值，布尔值为true，则把路由收到的所有params参数通过props传给Detail组件
    //props：true
    //第三种写法：props值为函数，该函数返回的对象中每一组key-value都会通过props传给Detail组件
    props(){
        return {
            id:$route.query.id,
            title:$route.query.title
        }
    }
}
```



### 8.<router-link>的replace属性

​	1.作用：控制路由跳转时操作浏览器历史记录的模式

​	2.浏览器的历史记录由两种写入方式：分别为push和replace，push是追加历史记录，replace是替换当前记录，路由跳转时候路径默认为push

​	3.如何开启replace模式：<router-link replace ></router-link>



### 9.编程式路由导航

​		1.作用：不借助<router-link>实现路由跳转，让路由跳转更加灵活

​		2.具体编码：

```javascript
//$router的两个API
this.$router.push({
    name:'xiangqing',
    params:{
        id:xxx,
        title:xxx
    }
})
this.$router.replace({
    name:'xiangqing',
    params:{
        id:xxx,
        title:xxx
    }
})

this.$router.forward() //前进
this.$router.back() //后退
this.$router.go()  //可前进也可后退

```



### 10.缓存路由组件

​		1.作用：让不展示的路由组件保持挂载，不被销毁

​		2.具体编码：

```html
<!--缓存一个路由组件-->
<keep-alive include="News">
    <router-view></router-view>
</keep-alive>
<!--缓存多个路由组件-->
<keep-alive :include="['News','Message']">
    <router-view></router-view>
</keep-alive>

```

### 11.两个新的生命周期钩子

​		1.作用：路由组件所独有的两个钩子，用于捕获路由组件的激活状态

​		2.具体名字：

​						1.activated路由组件被激活时触发

​						2.deactivated路由组件失活时触发

### 12.路由守卫

​	1.作用：对路由进行权限控制

​	2.分类：全局守卫，独享守卫，组件内守卫

​	3.全局守卫：

```javascript
//全局前置守卫，初始化时执行，每次路由切换之前执行
router.beforeEach((to,from,next)=>{
	console.log('beforeEach',to,from)
	if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制
        if(localStorage.getTtem('school') === 'atguigu'){ //权限控制的具体规则
            next()//放行
        }else{
            alert('暂无权限查看')
            //next({name:'guanyu'})
        }
	
	}else{
        next() //放行
    }
})


//全局后置守卫，初始化时执行，每次路由切换后执行
router.afterEach((to,from)=>{
    console.log('afterEach',to,from)
    if(to.meta.title){
        document.title = to.meta.title
    }else{
        document.title = 'vue_test'
    }
})
```

​		4.独享守卫：

```javascript
beforeEnter(to,from,next){
    console.log('beforeEnter',to,from)
    if(to.meta.isAuth){
        if(localStorage.getItem('school') === 'atguigu'){
            next()
        }else{
            alert('暂无权限查看')
            //next({name:'guanyu'})
        }
    }else{
        next()
    }
}
```

​		5.组件内守卫

```javascript
//进入守卫，通过路由规则，进入该组件时被调用
beforeRouteEnter(to,from,next){

},
//离开守卫，通过路由规则，离开该组件时被调用
beforeRouteLeave(to,from,next){

}
```



### 13.路由器的两种工作模式

​		1.对于一个url来说，什么是hash值？--------#及其后面的内容就是hash值

​		2.hash值不会包含在HTTP请求中，即：hash值不会带给服务器

​		3.hash模式：

​						（1）地址中永远带着  #  号 ，不美观

​						（2）若以后将地址通过第三方手机app分享，若app校验严格，则地址会被标记为不合法

​						（3）兼容性号好

​						  

​		4.history模式：

​						（1）地址感干净，美观

​						（2）兼容性和hash模式相比略差

​						（3）应用部署上线时需要后端人员支持，解决刷新页面服务端404的问题

### 14.路由传递参数

```javascript
第一种：字符串形式
this.$router.push("/search/"+this.keyword+"?k="+this.keyword.toUpperCase());
第二种：模板字符串
this.$router.push(`/search/${this.keyword}?k=${this.keyword.toUpperCase()}`);
第三种：对象
this.$router.push({
    name:'search',
    params:{
        keyword:this.keyword
    },
    query:{
        k:this.keyword.toUpperCase()
    }
})
```

### 15.重写push|replace方法

解决重复点击跳转问题

```javascript
//    router/index.js

let originPush = VueRouter.prototype.push;
//重写push|replace
//第一个参数：告诉原来push方法，你往哪里跳转（传递哪些参数）
//第二个参数：成功回调
//第三个参数：失败回调
//call||apply区别
//相同点，都可以调用函数一次，都可以篡改函数的上下文一次
//不同点：call与apply传递参数，call传递参数用逗号隔开，apply方法执行，传递数组
VueRouter.prototype.push = function(location,resolve,reject){
    if(resolve&&reject){
     
        originPush.call(this,location,resolve,reject);

    }else{
        originPush.call(this,location,()=>{},()=>{});
    }
}

VueRouter.prototype.replace = function(location,resolve,reject){
    if(resolve&&reject){
        originPush.call(this,location,resolve,reject);

    }else{
        originPush.call(this,location,()=>{},()=>{});
    }
}

```



## 面试题：

### 1.路由传递参数（对象写法）path是否可以结合params参数一起使用？

答：路由跳转传参的时候，对象的写法可以是name、path形式，但是需要注意的是，path这种写法不能与params参数一起使用

### 2.如何指定params参数可传可不传？

如果路由要求传递params参数，但是你就不传递params参数，发现一件事情，url会有问题

如何指定params参数可以传递、或者不传递，在配置路由的时候，在占位的后面加上一个问号【代表params参数可以传递或者不传递】

### 3.params参数可以传递也可以不传递，但是如果传递的是空串，如何解决？

使用undefined解决：params参数可以传递、不传递（空的字符串）

```javascript
this.$router.push({
	name:'search',
    params:{
        keyword:''||undefined
    },
    query:{
        k:this.keyword.toUpperCase()
    }
})
```



## 基础函数语法

###### 随机取数字

```
Math.random()      #区间默认[0,1),包含0，但不包含1

Math.random()*3    #区间[0,3)，包含0，但不包含3，会取到小数

Math.floor(Math.random()*3)   区间[0,3),取整

```

###### 操作数组

对于数组的操作，不能直接通过索引，例如：this.arr[0]=10（不奏效）

```
arr.shift()  		//移除数组的第一个元素
arr.push('aaa') 	//往数组里添加元素'aaa'
arr.pop          	//删除最后一个元素
arr.shift 			//删除第一个元素
arr.unshift('aaa')	//在首位添加元素'aaa'
arr.sort    		//排序
	
arr.splice(index)  //1.当index>=0 从index的位置开始，删除之后的所有元素，包括第index个
					 2.当index<0	则删除最后index个元素，splice函数返回删除元素数组
					 （会改变原数组）
					 
arr.splice(index,howmany)  //删除从index位置开始的数，howmany为删除的个数
							若howmany<=0，则不删除
arr.reserve()         		//返回颠倒后的数组
```

###### **过滤数组函数   filter**

filter：用于对数组的过滤，它创建一个新数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。不会对空数组进行检测，不会改变原始数组

```
watch:{
	keyWord(Val){   //keyWord被监听的数据   Val是值
		this.arr.filter((p)=>{  
			return p.name.indexOf(Val) !== -1  //在对象中查找val并返回索引，找不到返回-1
		})
	}
}
 ####   string 中 indexOf()会将数值参数转换为字符再查询索引
 ####   number不可用indexOf，会报错
 ####   array 中 indexOf()是严格比较
！！！    注意：
 			name：'abcd'
 			this.name.indexOf('') ==0   （ ！！！！不等于-1）
 			查找空字符串，返回值为0
```

###### 排序函数  sort

```
let arr[1,5,3,4,6,0]
//升序（前减后）
arr.sort((a,b)=>{
	return a-b 
})
//降序（后减前）
arr.sort((a,b)=>{
	return b-a
})
```

## 实用小插件

### nprogress（浏览器顶部进度条插件）

```javascript
npm install --save nprogress

import axios from 'axios';

//引入进度条
import nprogress from "nprogress";
//引入进度条样式
import "nprogress/nprogress.css" ;
//start：进度条开始 done：进度条结束
const requests = axios.create({
    baseURL:"/api",
    timeout:5000
})

//请求拦截器：再发请求之前，请求拦截器可以检测到，可以在请求发出去之前做一些事情
requests.interceptors.request.use((config)=>{
    
    //进度条开始动
    nprogress.start();
    return config;
})

//响应拦截器：
requests.interceptors.response.use((res)=>{
    //进度条结束
    nprogress.done();
    return res.data;
},(error)=>{
    
    return Promise.reject(new Error('faile'));
})

export default requests;
```

配合请求拦截器和响应拦截器使用

**进度条开始：nprogress.start()**

**进度条结束：nprogress.done()**