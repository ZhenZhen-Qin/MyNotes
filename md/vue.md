# vue 

## 一、简介：

​	angular chrome团队开发   效率低  双向数据开发

​	react   Facebook团队开发  虚拟DOM



vue全家桶：vue-cli    vue-router（监听页面 的hash变化）     vuex    axios（post，请求拦截器，响应拦截器）    vux(是一个UI框架)



1. 渐进式框架
   1. 自底向上逐层应用
   	. 现代化的工具链  webpack打包 npm安装	
2. 安装：
   1. 直接引入  
   2.  vue-cli  脚手架工具安装
      1. 全局安装vue-cli 工具  【 npm install  vue-cli -g  】
   3. ![1541988337704](C:\Users\覃珍珍\AppData\Local\Temp\1541988337704.png)



![1541988470063](C:\Users\覃珍珍\AppData\Local\Temp\1541988470063.png)



![1541988483512](C:\Users\覃珍珍\AppData\Local\Temp\1541988483512.png)

#### VUE中实现MVVM以及响应式数据绑定的原理（响应式原理）

        // 在vue中存在着很多的指令（v-）,这些指令挂载在视图上的时候，当我们操作这个视图中dom的时候，viewmodel就会得知，渲染数据的时候，在视图上放上表达式，这样当viewmodel要更新视图的时候，表达式里的数据也会改变，所以实现了VM与V的关系
    
        //vue在创建viewmodel的时候会将数据配置在实例中，然后会使用Object.defineProperty对这些数据进行处理，给他们添加上专门的getter和setter以及watcher，这样当数据改变了之后，就会触发这条数据的setter，从而触发viewmodel中对应的watcher，然后viewmodel再去更新视图
    
        //这就是VUE中实现MVVM以及响应式数据绑定的原理（响应式原理）


        //响应式原理：在vue中会对数据通过Object.defineProperty进行处理，生成对应的getter和setter，当数据更改的时候会触发对应的setter，这个时候vue中的watcher会去更改视图

指令（v-...）

代码：

```
	<div id="app">
        <input type="text" v-model="message">
        <p>{{message}}</p>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        let data = {
            message: 'Hello World'
        }
        new Vue({
            data: data
        })

        let _data = {}

        let middle = 'hello'
        Object.defineProperty(_data,'message',{
            get () {
                return middle
            },
            set (val) {
                middle = val
                //执行对应的watcher，让viewmodel去更新视图
            }
        })
        _data.message = 'hello world'
    </script>
```



## 二、vue 开始开发  

【数据驱动  数据绑定 双向绑定 mvvm】    减少开发者操作DOM , 增加对数据的处理 ， 通过某一个联系，将view（页面）层 和model（数据层） 关联在一起，数据变视图变    【联系：指令  v-model  v-for 建立联系】

声明式渲染：数据的单项绑定

-------------------------------------------------------------------------------------------------------------------



![1542002875054](C:\Users\覃珍珍\AppData\Local\Temp\1542002875054.png)

1.  数据绑定

   ```
   new Vue({
       el:"#app",  //数据的作用范围，如作用在id为app的元素，只能是body里面的元素
       data:{
           name:"123",
           age:13
       }
   })
   ```

   



备注：  【 vue 没有操作DOM的节点一说法，数据变化，页面就自动变化】 

【vue中的事件默认参数是event对象,如果有其他参数，第二根参数是$event】



#### 组件式开发：  （组件的应用：页面有重复的东西-->组件）

1. 将页面进行划分成组件，需要的时候进行调用 【组件就是vue 实例的一个子类】

2. 创建组件  Vue.extend  【组件构造器    分为全局组件，局部组件,组件的数据要求是一个函数，在函数内部返回一个数据】

   全局组件  【注册于Vue  所有的Vue实例都可以使用】

   ```
   第一步： 创建组件  
   let componentName = Vue.extend({template:'<h1></h1>'})  //页面展示的是渲染模板里面的东西
   
   使用：
   第二步：注册组件
   Vue.component("hehe",componentName)  
   参数1：注册的组件名    参数2：绑定的组件名
   
   第三步： 在body里面去使用
   <hehe></hehe>
   
   然后在页面上使用 
   【html代码中的东西都是假的，最终以编译结果为主l】
   
   简写方式
   Vue.component("hehe",{template:'<h1></h1>'})  
   使用：<hehe></hehe>
   ```

   局部组件  【注册于vue实例 只有实例内部可以使用】

   ```
   创建 ：let heheName = Vue.extend({template:'<h1></h1>'})  
   注册 ：let vm1 = new Vue({
   	...
       compponents:{"hello-word":heheName}
   })
   
   使用 ： <hello-word></hello-word>
   ```

3.   is 的用法

   ![1542075436807](C:\Users\覃珍珍\AppData\Local\Temp\1542075436807.png)

   提供的自定义标签![1542075500177](C:\Users\覃珍珍\AppData\Local\Temp\1542075500177.png)

4.   spa   [singer page application]  单页面应用

   父子组件的嵌套管理    在全局组件中，父子关系只是存在于嵌套的关系

   ![1542075683597](C:\Users\覃珍珍\AppData\Local\Temp\1542075683597.png)

   

5. 组件通信  【组件交互】

   1. 【在子组件里 通过Props 抛出一个数据接口，将作为数据的自定义属性使用，接受的时候通过自定义属性进行接受，数据流向是单项的，从父组件到子组件流动】

   2. 通过父组件方法修改父组件的值。

      

      父组件传给子组件：  在子组件添加跑【 props:["demo"] 】,    在父组件使用【 <son :demo="Num"></son> 】

      ```js
      <body>
      	<div id="app">
      		<father></father>		
      	</div>
      <!-- 组件模板应该包含一个根元素。如果在多个元素上使用V-IF，则使用V-ELS-IF来链接它们。 -->
      	
      	<template id="father">
      		<div>
      			<button @click='add'>add</button>
      	        {{Num}}
      	         <hr>
      	         <son :demo="Num"></son>
      		</div>
      	</template>
      	
      
      	<template id="son">
      		<div>
      			<p>子组件{{demo}}</p>
      		</div>
      	</template>
      	
      	<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
      	<script>
      		Vue.component("father",{
      			template:"#father",
      			data(){
      				return {
      					Num:1
      				}
      			},
      			methods:{
      				add(){
      					this.Num++
      				}
      			}
      		}).component("son",{
      			template:"#son",
      			props:["demo"],
      			data(){
      				return {
      					Num:2
      				}
      			}
      		})
      
      
      		let vm = new Vue({
              	el: '#app'
          	})
      	</script>
      	
      </body>
      </html>
      ```

      子组件控制父组件  【通过第一种方法传递一个函数，函数传一个参数】

      ```js
      <body>
          <div id="app">
              <father></father>
          </div>
        
          <template id="father">
              <div>
                  <p>这里是父组件</p>
                  {{num}}
                  <button @click='fadd'>fadd</button>
                  <hr>
                  <son :callback='fadd'></son>
              </div>
          </template>
      
          <template id="son">
              <div>
                   <p>这里是子组件</p>
                   <button @click='add'>add</button>
                    {{num}}
                    {{callback}}
              </div>
          </template>
      
      </body>
      <script src="../../../vue.js"></script>
      <script>
           new Vue({
              el:'#app',
              components:{'father':{
                  template:'#father',
                  data(){
                      return {
                          num:2
                      }
                  },
                  methods:{
                      fadd(val){
                          this.num+=val
                      }
                  },
                  components:{'son':{
                      template:'#son',
                      data(){
                          return {num:0}
                      },
                      props:['callback'],
                      methods:{
                          add(){
                              this.num++
                              //修改子组件的num
                              this.callback(2)
                              //调用父组件的方法 修改父组件的数据
                          }
                      }
                  }}
              }}
           })
      //  2个组件嵌套   子组件有一个button  父组件有一个num   点击子组件触发父组件
      //  通过在子组件传递一个回调函数的方式
      //  通过父组件的方法修改父组件的值   将父组件的方法通过 props 传递给子组件 然后由子组件进行调用
          // 2个组件不嵌套 兄弟  一个有nun  一个有button  点buttom  num++
          // 组件通信
      </script>
      </html>
      ```

      

6. 

7. 



#### Vue 的生命周期

1. beforeCreate
2. ![1542160131096](C:\Users\覃珍珍\AppData\Local\Temp\1542160131096.png)

```

```

![1542182326850](C:\Users\覃珍珍\AppData\Local\Temp\1542182326850.png)





## Vuex   

state  存放所有的全局状态值（不是全局变量）   

​	





解决问题：多组件共享状态：比如传参







基本使用：

 1.登录的状态管理

 2.显示隐藏效果





