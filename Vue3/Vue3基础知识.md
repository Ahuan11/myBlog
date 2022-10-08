# 一、常用 Composition API

## 1.Vue3.0的响应式

**实现原理：**

  1. 通过  Proxy（代理）：拦截对象中任意属性的变化，包括：属性值的读写、属性的添加、属性的删除等。

  2. 通过  Reflect（反射）：对源对象的属性进行操作。

  3. MDN文档中描述的  Proxy 与Reflect：

     ​	Proxy：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy

     ​	Reflect：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect

     ```javascript
     new Proxy(data,{
     	//拦截读取属性值
     	get(target,prop){
     		return Reflect.get(target,prop)
     	},
     	//拦截设置属性值或添加新属性
         set(target,prop,value){
             return Reflect.set(target,prop,value)
         },
         //拦截删除属性
         deleteProperty(target,prop){
             return Reflect.deleteProperty(target,prop)
         }
     })
     proxy.name = 'tom'
     ```

     

## 2.reactive对比ref

1. 从定义数据角度对比：

​		**ref**用来定义：**基本类型数据**

​		reactive用来定义：**对象（或数组）类型数据**

		>**备注**：ref也可以用来定义**对象（或数组）类型数据**，它内部会自动通过**reactive**转为代理对象

​	2.从原理角度对比：

​		**ref**通过**Object.defineProperty()**的**get**和**set**来实现响应式（数据劫持）

​		**reactive**通过使用**Proxy **来实现响应式（数据劫持），并开通**Reflect**操作源对象内部的数据

​	3.从使用角度对比：

​		**ref**定义的数据：操作数据需要**.value**，读取数据时模板中直接不需要**.value**

​		**reactive**定义的数据：操作数据与读取数据，都不需要**.value**

## 3.setup的两个注意点

- ​	执行的时机

  ​			在beforeCreate之前执行一次，this是undefined

- ​	setup的参数

​			props：值为对象，包含：组件外部传递过来的但没有在props配置中声明的属性，相当于this.$attrs。

​			context:上下文对象

​					attrs：值为对象，包含：组件外部传递过来，但没有在props配置中声明的属性，相当于this.$attrs

​					slots：收到的插槽内容，相当于this.$slots。

​					emit：分发自定义事件的函数，相当于this.$emit。

## 4.watch函数

- 与Vue2.x中watch配置功能一致

- 两个小坑：

  - 监视reactive定义的响应式数据时，oldValue无法正确获取，强制开启了深度监视（deep配置失效）

  - 监视reactive定义的响应式数据中某个属性时：deep配置有效。

    ```javascript
    //情况一：监视ref定义的响应式数据
    watch(sum,(newValue,oldValue)=>{
        console.log('sum变化了',newValue,oldValue)
    },{immediate:true})
    
    //情况二：监视多个ref定义的响应式数据
    watch([sum,msg],(newValue,oldValue)=>{
        console.log('sum或msg变化了',newValue,oldValue)
    })
    
    //情况三：监视reactive定义的响应式数据
    //		若watch监视的是reactive定义的响应式数据，则无法正确获得oldValue
    //		若watch监视的是reactive定义的响应式数据，则强制开启了深度监视
    watch(person,(newValue,oldValue)=>{
        console.log('person变化了',newValue,oldValue)
    },{immediate:true,deep:false})  //此处的deep配置不在奏效
    
    //情况四：监视reactive定义的响应式数据中的某个属性
    watch(()=>person.job,(newValue,oldValue)=>{
        console.log('person的job变化了',newValue,oldValue)
    },{immediate:true,deep:false})
    
    
    //情况五：监视reactive定义的响应式数据中的某些属性
    watch([()=>person.name,()=>person.age],(newValue,oldValue)=>{
        console.log('person的name或age变化了',newValue,oldValue)
    })
    
    //特殊情况
    watch(()=>person.job,(newValue,oldValue)=>{
        console.log('person的job变化了',newValue,oldValue)
    },{deep:true})//此处由于监视的是reactive所定义的对象中的某个对象属性，所以deep配置奏效
    ```

    

## 5.watchEffect函数

- watch的套路是：既要指明监视的属性，也要指明监视的回调

- watchEffect的套路是：不用指明监视哪个属性，监视的回调中用哪个属性，那就监视哪个属性

- watchEffect有点像computed：

  - 但computed注重的是计算出来的值（回调函数的返回值），所以必须要写返回值

  - 而watchEffect更注重的是过程（回调函数的函数体），所以不用写返回值

    ```javascript
    //watchEffect所指定的回调中用到的数据只要发生变化，则直接重新执行回调
    watchEffect(()=>{
    	const x1 = sum.value
        const x2 = person.age
        console.log('watchEffect配置的回调执行了')
    })
    ```

    

## 6.自定义hook函数

- 什么是**hook**？ -------本质是一个函数，把setup函数中使用的Composition API进行了封装
- 类似于vue2.x中的mixin
- 自定义**hook**的优势：复用代码，让setup中的逻辑更清楚易懂

## 7.toRef

- 作用：创建一个ref对象，其value值只想另一个对象中的某个属性
- 语法：const name = toRef(person,‘name’) 
- 应用：要将响应式对象中的某个属性单独提供给外部使用时
- 扩展   toRefs 与  toRef  功能一致，但可以批量创建多个ref对象，语法：toRefs(person)

# 二、其它 Composition API

## 1.shallowReactive 与 shallowRef

- shallowReactive ：只处理对象最外层属性的响应式（浅响应式）。
- shallowRef ：只处理基本数据类型的响应式，不进行对象的响应式处理
- 什么时候使用？
  - 如果有一个对象数据，解构比较深，但变化时知识外层属性变化 ===> shallowReactive
  - 如果有一个对象数据，后续功能不会修改该对象中的属性，而是生新的对象来替换 ===> shallowRef

## 2.readonly 与 shallowReadonly

- readonly：让一个响应式数据变为只读的（深只读）
- shallowReadonly：让一个响应式数据变为只读的（浅只读）。
- 应用场景：不希望数据被修改时。

## 3.toRaw 与 markRaw

- **toRaw**：
  
  - 作用：将一个由reactive生成的**响应式对象**转为**普通对象**
- 使用场景：用于读取响应式对象对应的普通对象，对整个普通对象的所有操作，不会引起页面刷新、
  
- **markRaw**：
  
  - 作用：标记一个对象，使其永远不会在成为响应式对象。
  - 应用场景：
    1. 有些值不应该设置为响应式的，例如复杂的第三方类库等。
    2. 当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能

## 4.customRef

- 作用：创建一个自定义的ref，并对其依赖项跟踪和更新触发进行显示控制

- 实现防抖效果：

  ```vue
  <template>
  	<input type="text" v-model="keyword">
  	<h3>{{keyword}}</h3>
  </template>
  
  <script>
      import {ref,customRef} from 'vue'
      export default {
          name:'Demo',
          setup(){
              //let keyword = ref('hello')  //使用Vue准备好的内置ref
              //自定义一个myRef
              function myRef(value,delay){
                  let timer
                  //通过customRef去实现自定义
                  return customRef((track,trigger)=>{
                      return{
                          get(){
                              track() //告诉vue整个value值是需要被追踪的
                              return value
                          },
                          set(newValue){
                              clearTimeout(timer)
                              timer = setTimeout(()=>{
                                  value = newValue
                                  trigger() //告诉vue去更新
                              },delay)
                          }
                      }
                  })
              }
              let keyword = myRef('hello',500)
              return  {keyword}
          }
      }
  </script>
  ```

  

## 5.provide 与 inject

- 作用：实现**祖与后代组件**间通信

- 套路：父组件有一个provide选项来提供数据，后代组件有一个inject选项来开始使用这些数据

- 具体写法：

  1. 祖组件中：

     ```vue
     setup(){
     	let car = reactive({name:'奔驰',price:'40万'})
     	provide('car',car)
     }
     ```

     2. 后代组件中：

        ```
        setup(props,context){
        	const car = inject('car')
        	return {car}
        }
        ```

## 6.响应式数据的判断

- isRef：检查一个值是否为ref对象
- isReactive：检查一个对象是否为reactive创建的响应式代理
- isReadonly：检查一个对象是否是由readonly创建的只读代理
- isProxy：检查一个对象是否由reactive或者readonly方法创建的代理

# 三、新的组件

## 1.Fragment

- 在Vue2中：组件必须有一个根标签

- 在Vue3中：组件可以没有根标签，内部会将多个标签包含在一个Fragment虚拟元素中

- 好处：减少标签层级，减少内存占用

  

## 2.Teleport

- 什么时Teleport？-------Teleport是一种能够将我们的组件html结构移动到指定位置的技术

  ```vue
  <teleport to="移动位置">
      <div v-if="isShow" class="mask">
         <div class="dialog">
             <h3>我是一个弹窗</h3>
             <button @click="isShow = false">关闭弹窗</button>
          </div>
      </div>
  </teleport>
  ```

  

## 3.Suspense

- 等待异步组件时渲染一些额外内容，让应用有更好的用户体验

- 使用步骤：

  - 异步进入组件

    ```javascript
    import {defineAsyncComponent} from 'vue'
    const Child  = defineAsyncComponent(()=>import('./components/Child.vue'))
    ```

  - 使用Suspense包裹组件，并配置好default 与 fallback

    ```vue
    <template>
    	<div class="app">
            <h3>我是App组件</h3>
            <Suspense>
                <template v-slot:default>
                    <Child/>
    			</template>
    			<template v-slot:fallback>
                    <h3>稍等，加载中。。。</h3>
    			</template>
        	</Suspense>
        </div>
    </template>
    ```

    

# 四、其他

## 1.全局API的转移

- vue2.x有许多全局API和配置

  - 例如：注册全局组件、注册全局指令等

    ```vue
    //注册全局组件
    vue.component('MyButton',{
    	data:()=>({
    		count:0
    	}),
    	template:'<button @click="count++">Clicked {{count}} times.</button>'
    })
    //注册全局指令
    Vue.directive('focus',{
    	inserted:el=>el.focus()
    })
    ```

    

- vue3.0中对这些API做出了调整：

  - 将全局的API，即：Vue.xxx调整到应用实例（app）上

    | 2.x全局API（Vue）        | 3.x实例API（app）           |
    | ------------------------ | --------------------------- |
    | Vue.config.xxx           | app.config.xxx              |
    | Vue.config.productionTip | 移除                        |
    | Vue.component            | app.component               |
    | Vue.directive            | app.directive               |
    | Vue.mixin                | app.mixin                   |
    | Vue.use                  | app.use                     |
    | Vue.prototype            | app.config.globalProperties |

    