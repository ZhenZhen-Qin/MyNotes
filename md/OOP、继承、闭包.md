# 一、面向对象

利用对象进行编程的一种思想。object-oriented programming，简称oop。

## （一）面向过程及面向对象的区别

###### 举例:五子棋游戏

```
//面向过程：
1、开始游戏，
2、黑子先走，
3、绘制画面，
4、判断输赢，
5、轮到白子，
6、绘制画面，
7、判断输赢，
8、返回步骤2，
9、输出最后结果。
//面向对象
1、黑白双方，这两方的行为是一模一样的.  位置、种类{black:[],white:[]}
2、棋盘系统，负责绘制画面。盘、黑白子、[创建span，添加不同类名，位置。]
3、规则系统，负责判定诸如犯规、输赢等。判断是不是在同一条线上
//总结:面向对象是按照功能划分问题，保证了可扩展性。例如添加悔棋功能，如果是面向过程，那么从输入到判断到显示这一连串的步骤都要改动，甚至步骤之间的循序都要进行大规模调整。而面向对象，只需要让棋盘系统回溯到上一步即可，无需纠结1、3功能。
```

###### 演示：创建并描述对象

- 描述一个人
- 描述购物车

## （二）如何创建对象

#### 1.字面量  【用于创建单个对象】

var student = {id:10,name:'小明',age:18} 

#### 2.通过new关键字实例化对象  【用于创建单个对象】

```
var student = new Object()
student.id = 10;
student.name = '王铁锤';
student.age = 18;
//缺点：使用同一个接口创建很多对象，会产生大量的重复代码
```

##### 工厂模式（可以创建多个对个对象、产生的对象的构造函数都是Object，类不明确）

在ECMAScript中是无法创建类的，开发人员就发明了一种函数，用函数来封装特定接口创建对象的细节。

```
function createPerson(name, age, job) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    sayName = function () {
        alert(this.name);
    };
    return o;
}
var person1 = createPerson('zxj', 23, "Software Engineer");
var person2 = createPerson('sdf', 25, "Software Engineer");

//不足：在示例中我们可以看到，工厂模式虽然解决了创建多个相似对象的问题，但没有解决对象识别的问题（在示例中，得到的都是o对象，对象的类型都是Object）。
```

#### 3.自定义构造函数（类的概念）  【常用】

ECMAScript中的构造函数可以用来创建特定类型的对象。像Object和Array的原生的构造函数，在运行时会自动出现在执行环境中。此外，也可以创建自定义的构造函数，从而定义自定义对象类型的属性和方法。

###### 演示：自定义构造函数相对于工厂函数的好处

```
function Student(name, age){
    this.name = name;
    this.age = age;
    this.sayName = function(){alert(this.name);};
}
var s1 = new Student("王铁锤", 18);
//可以通过控制台查看一下person1与s1的区别，更好理解。
```

#### 构造函数与普通函数的区别

> 唯一区别：调用方式不同

- 任何函数，只要通过new操作符来调用，它就可以作为构造函数；
- 而任何构造函数，如果不通过new 操作符来调用，那它跟普通函数无区别。

> 不成文的约定：构造函数名首字母大写

###### 演示：构造函数与普通函数执行方式的区别

```
function Dog(){
	this.name = '哈巴';
	this.color = '白色';
	this.jiao = function(){
    	console.log("我的名字叫"+this.name);
    }
}
// 普通函数执行方式
// Dog(); => 没有值return出来，所以输出结果为undefined
// 构造函数的调用方式
// new Dog(); => 输出结果为对象
```

#### * 调用自定义构造函数实际上会经历以下4个步骤：

1. 创建一个新对象( 在内部隐式调用了new Object() )；

2. 将构造函数的作用域赋给新对象（把this绑定到实例对象）；

3. 执行构造函数中的代码（为这个新对象添加属性）；

4. 返回新对象。

   ![1539133507930](C:\Users\覃珍珍\AppData\Local\Temp\1539133507930.png)

```
// 通过new调用函数==>构造函数
// 1. var obj = {}
// 2. Dog.bind(obj)
// 3. 执行构造函数中的代码：this为obj
// 4. return obj;
```

## （三）this 的指向问题 

函数中的this作为JS的关键字，有特殊的含义，代表了当前对象，而当前对象是谁，由函数执行时所处的环境来决定。

<!--this好比一句话, 出自不同人之口, 代表的人就不一样-->
<!--如A和B吵架：-->
<!--A对B说: “老子要砍死你! ” , 这里的老子指A-->
<!--B对A说: “老子要弄死你! ”, 这里的老子指B-->

用new关键字执行：this指向生成的实例对象
普通函数执行：this指向调用函数的对象

注：在js的整理笔记中有详细的介绍

## （四）对象的组成部分：（构造函数、实例对象、原型对象）

#### 构造函数

- this指向实例对象

#### 实例对象

1. 用new关键字生成的对象称为实例，实例会复制构造函数内所有的属性和方法。
2. 可以实例对象获取原型对象：

```
__proto__  //Frefox,Chrome等浏览器可以通过私有属性
Object.getPrototypeOf(实例)； //ES5方式去获取
```

#### 原型对象

- 我们创建的每个函数都有一个prototype属性，指向原型对象。
- 原型对象默认包含一个constructor属性，指向构造函数。
- **任何写在原型对象中的属性和方法，都可以让所有实例对象共享。**

###### 演示：画图演示三者的关系

![](C:\HTML5-1808\25-27.OOP&Inherit、闭包\doc\神笔马良\01.对象三大组成之间的关系.png)

#### 三者的关系

1. 每个构造函数都有一个原型对象（prototype），
2. 原型对象都包含一个指向构造函数的指针（constructor），
3. 而所有实例都通过`__proto__`指向同一个原型对象。（包含一个指向原型对象的内部指针（[[prototype]]））

##### 判断原型和实例的关系（返回布尔值）

constructor: 得到构造函数的引用，一般用于判断该实例是否由某一构造函数生成

```
实例.constructor == Student //true
```

instanceof: 检测某个对象是不是某一构造函数的实例，适用于原型链中的所有实例

```
实例 instanceof Student //true
实例 instanceof Object //true
```

isPrototypeOf: 判断当前对象是否为实例的原型对象

```
原型对象.isPrototypeOf(实例) //true
```

## （五）实际应用

构造函数方法很好用，但是单独使用存在一个浪费内存的问题（所有的实例会复制所有构造函数中的属性/方法）。这样既不环保，也缺乏效率。

#### **解决方案:构造函数+原型对象**

- 使用构造函数添加私有属性
- 使用原型对象添加共享方法

**优点：**

- 实例对象都有自己的独有属性
- 实例共享了原型中的方法，最大限度的节省了内存
- 支持向构造函数传递参数

###### 演示：使用原型对象添加共享方法

```
function Person(name,age,gender){
    // 属性
    this.name = name;
    this.age = age;
    this.gender = gender;
    // 方法
    // this.say = function(){console.log('say');}
    // this.singing = function(){console.log('singing');}
}
 // 把方法提取到原型对象
 Person.prototype.say = function(){
    console.log('say');
 }
 Person.prototype.singing = function(){
    console.log('singing');
 }
 var lx = new Person('laoxie',18,'male');//2M	
 var lm = new Person('lemon',31,'femal');//5M	
```

###### 案例：弹幕效果

```
//1.生成页面元素对象
//属性有：整个弹幕区域、消息框、按钮
//方法有：
//	-初始化：获取元素，点击按钮，执行发送方法
//	-发送方法，获取消息框内容。实例化弹幕对象
let page = {
    ele:'#barrage',
    msg:'#msg',
	button:'#msg+button',
    init(){
        this.ele = document.querySelector(this.ele);
        this.msg = document.querySelector(this.msg);
        this.btn = this.msg.nextElementSibling;
        this.btn.onclick = ()=>{
            this.send();
        }
    },
    send(){
        let msg = this.msg.value;
        new Barrage(msg);
    }
}
2.自定义弹幕对象的构造函数
(1) 属性：内容、颜色、速度、字体大小、位置
(2) 方法：
    - 初始化init()：创建弹幕对象this.ele，添加内容类名及样式，添加到元素内
    - 运动move()：从右往左
    - 移除remove():移除弹幕
function Barrage(msg){
    this.color = randomColor();
    this.speed = randomNumber(-20,-5);
    this.size = randomNumber(12,48);
    this.position = randomNumber(10,page.ele.clientHeight-this.size-10);
    this.init(msg);
}
Barrage.prototype.init = function(msg){
    // 创建弹幕元素
    this.ele = document.createElement('span');
    this.ele.innerText = msg;
    this.ele.className = 'bar-item';
    // 定义样式
    this.ele.style.color = this.color;
    this.ele.style.fontSize = this.size + 'px';
    this.ele.style.top = this.position + 'px';
    // 写入页面
    page.ele.appendChild(this.ele);
    // 移动
    this.move();
}
Barrage.prototype.move = function(){
    this.timer = setInterval(()=>{
        let left = this.ele.offsetLeft;
        left += this.speed;
        if(left <= -this.ele.offsetWidth){
            clearInterval(this.timer)
            this.remove();
        }
        this.ele.style.left = left + 'px';
    },30);
}
Barrage.prototype.remove = function(){
    this.ele.parentNode.removeChild(this.ele);
}
// 页面元素对象的init方法开始执行
page.init();
```

###### 案例：烟花效果

```
* 1.页面 page （字面量：一个）
	* 属性： 按钮、显示区域
	* 方法： 初始化（获取元素，给this.ele绑定点击事件,获取光标位置，实例化烟花对象）
let page = {
    ele:'body',
    btn:'#auto',
    init(){
        this.ele = document.querySelector(this.ele);
        this.btn = document.querySelector(this.btn);
        document.onclick = function(e){console.log(666)
            new Firework(e.pageX,e.pageY);
        }
    }
}	
* 2.烟花 firework
	* 属性：位置、爆炸数量、爆炸半径
	* 方法：
		（1）初始化：生成span，添加类名，及当前的水平坐标，添加到页面中。执行移动方法
		（2）移动： 利用动画移动到光标top位置，同时将自身高度变小。执行爆炸方法，同时移除自身。
		（3）爆炸：利用数量计算角度，将圆心位置，半径及角度都传给火花实例对象
		（4）移除
function Firework(x,y){
    this.left = x;
    this.top = y;
    // 爆炸数量
    this.qty = randomNumber(12,36);
    // 爆炸半径
    this.r = randomNumber(50,200);
    this.init();
}
Firework.prototype.init = function(){
    this.ele = document.createElement('span');
    this.ele.className = 'fire';
    this.ele.style.left = this.left + 'px';
    page.ele.appendChild(this.ele);
    this.move();
}
Firework.prototype.move = function(){
    animate(this.ele,{top:this.top,height:this.ele.clientWidth},()=>{
        this.boom();
        this.remove();
    })
}
Firework.prototype.boom = function(){
    for(var i=0;i<this.qty;i++){
        // 计算角度
        let deg = 360/this.qty*i;
        new Spark(this.left,this.top,this.r,deg);

    }
}
Firework.prototype.remove = function(){
    this.ele.parentNode.removeChild(this.ele);
}
* 3.爆炸烟火 spark	
	* 属性：随机色、半径、弧度、圆心位置
	* 方法：
		初始化：创建this.ele火花span、添加类名，设置初始化位置，写入页面
		火花移动:设置最终位置，利用动画移动到指定left、top值，及改变透明度。动画完成后移除
		移除火花
function Spark(x,y,r,deg){
    this.color = randomColor();
    this.r = r;
    this.rad = Math.PI*deg/180;
    this.init(x,y);
}
Spark.prototype.init = function(x,y){
    this.ele = document.createElement('span');
    this.ele.className = 'spark';
    // 设置颜色
    this.ele.style.backgroundColor = this.color;
    // 设置初始位置
    this.ele.style.left = x + 'px';
    this.ele.style.top = y + 'px';
    // 写入页面
    page.ele.appendChild(this.ele);
    this.move(x,y)
}
Spark.prototype.move = function(x,y){
    var a = this.r * Math.cos(this.rad);
    var b = this.r * Math.sin(this.rad);
    var left = parseInt(x + b);
    var top = parseInt(y - a);
    animate(this.ele,{left,top,opacity:0.3},()=>{
        this.remove();
    })
}
Spark.prototype.remove = function(){
	this.ele.parentNode.removeChild(this.ele);
}
// 操作对象
page.init()；
```



## （六）属性特性:ES5对象扩展(了解)

#### 值属性的属性特性

- configurable
  - 可配置性，控制着其描述的属性的修改，表示能否修改属性特性
- enumerable
  - 可枚举性，表示能否通过for-in遍历得到属性
- writable
  - 可写性，表示能否修改属性的值
- value
  - 数据属性，表示属性的值。默认值为undefined

#### 与属性特性相关的方法

```
设置属性特性：

Object.defineProperty(obj, property, descriptor) 给对象的某个属性设置属性特性
Object.defineProperties(object, descriptors) 给对象的所有属性设置属性特性

获取属性特性：

Object.getOwnPropertyDescriptor(object, propertyname)

获取对象的所有属性：

Object.keys(object) 只获取到可枚举的属性
Object.getOwnPropertyNames(object) 获取所有属性的名称（包含不能枚举的属性）
```

###### 演示：添加属性的相关方法（同时设置属性特性）

```
var obj = {
    a:1,
    b:"lemon"
}
//1.给对象添加一个属性，同时设置属性特性。此时其他的属性特性不设置默认都为false。
//即configurable为false，不可以修改属性特性。
//即enumerable为false，不可以枚举
//即writable为false，不可写，不可以修改属性值
Object.defineProperty(obj,"c",{value:"laoxie"})； //以上属性特性均可改成true，实现相应的功能。

// 2.同时修改多个属性特性
Object.defineProperties(obj, {
    b:{value:'css4',enumerable:false},
    c:{writable:true}  //报错，因为上面定义该属性的configurable为false，意思是不可以修改属性特性
})

//建议：用传统方式添加属性，利用defineProperty修改属性特性（configurable为true的前提下）
```



## （七）对象属性的遍历与判断

#### for…in

遍历对象中的所有可枚举属性, 无论该属性存在于实例中还是原型中

#### in

```
if(name in s1){
	//只要通过对象能够访问到属性就返回true, 无论该属性存在于实例中还是原型中
}
```

#### 对象.hasOwnProperty(属性)

- 检测一个属性是存在于对象本身中
  - 返回true，说明属性存在对象中
  - 返回false，说明属性不存在或在原型中

> 检测一个属性是否存在于原型中：!obj.hasOwnProperty(name) && (name in obj) 

## 重置原型对象

重置原型对象，可以一次性给原型对象添加多个方法，但切断了与原来原型对象的联系

```
function Popover(){}
Popover.prototype = {
    show:function(){},
    hide:function(){}
}
//- 注意覆盖问题（系统的原型对象不建议重写）
//- 注意识别问题（不可枚举性）
Object.defineProperty(Popover.prototype,"constructor",{value:"Popover",configurable:true})；
```

## 内置构造函数的原型对象

使用内置原型可以给已有构造函数添加方法

- 数组/字符串/数字等方法调用原理
- 扩展内置方法

###### 演示：对象的内置函数及扩展内置函数

```
if(!Array.prototype.norepeat){
    Array.prototype.norepeat = function(){
        return Array.from(new Set(this)); // this：指向实例arr
	}
}
```



# 二、闭包

##### 1.闭包的定义：

​	外层函数嵌套内函数，在外函数讲内函数返回出去，内部函数可以引用外部函数的参数和变量，参数和变量不会被垃圾回收机制所回收.

##### 2.垃圾返回机制：

​	当一个函数执行完毕时，并清除没有被对象引用的属性及方法

​	当一个函数执行完毕时，会清除内部的没有被引用的局部变量。全局变量不会被回收（尽量避免定义全局变量 ）

![1539267066634](C:\Users\覃珍珍\AppData\Local\Temp\1539267066634.png)

##### 3.闭包的用法：

​	在函数内部想使用函数内部的属性和方法【例如点击按钮获取对应的索引值】

异步应用 

参数和变量不会被垃圾回收机制所回收。

```
function aa(){
    var num = 10;
    function bb(){
        num++;
        console.log(num);
    }
    return bb;
}

//bb();  在这里无法访问内部的函数
var res = aa();   //此时res 接收到的是aa 的返回值 bb;

```

![1539239233882](C:\Users\覃珍珍\AppData\Local\Temp\1539239233882.png)

##### 4.闭包的好处：

1. 可以让一个变量长期驻扎在内存当中不被释放
2. 避免全局变量的污染, 和全局变量不同, 闭包中的变量无法被外部使用
3. 私有成员的存在, 无法被外部调用, 只可以自己内部使用

> 结论：

- 闭包是指有权访问另一函数作用域中的变量的函数
- 闭包，可以访问函数内部的局部变量，并让其长期驻留内存
- 由于闭包会携带包含它的作用域(运行环境)，因此会比其他函数占用更多内存，过度使用闭包可能会造成性能问题。

案例：

- 点击按钮打印当前索引值
- tab标签切换



三、原型链【实例与object原型对象之间的链条成为原型链】

​	（一）原型模式的访问机制（原型搜索机制）：	

1. 读取实例对象的属性时，先从实例对象本身开始搜索。如果在实例中找到了这个属性，则返回该属性的值；

2. 如果没有找到，则继续搜索实例的原型对象，如果在原型对象中找到了这个属性，则返回该属性的值

3. 如果还是没找到，则向原型对象的原型对象查找，依此类推，直到Object的原型对象（最顶层对象）；

4. 如果再Object的原型对象中还搜索不到，则抛出错误；

   （二）重置原型对象

   可以一次性给原型对象添加多个方法，但切断了与原来原型对象的联系 

   ```
   function Popover(){}
   Popover.prototype = {
       show:function(){},
       hide:function(){}
   }
   
   注意覆盖问题
   注意识别问题
   ```

   （三）内置原型对象

   使用内置原型可以给已有构造函数添加方法

   - 数组/字符串/数字等方法调用原理

   - 扩展内置方法

     

   ## 对象属性的遍历与判断

   1.for...in: 遍历对象中所有枚举属性，无论该属性存在于实例中还是原型中

   2.in:只要通过对象能够访问到的属性就返回true，无论该属性存在于实例中还是原型中

   ```
   if（name in s1）{
       
   }
   ```

   3. 对象.hasOwnProperty(属性)：检测一个属性是存在于对象本身中。	

      - 返回true，说明属性存在对象中

      - 返回false，说明属性不存在或在原型中

        ```
        检测一个属性是否存在于原型中：!obj.hasOwnProperty(name) && (name in obj)
        ```

        





# 继承

继承是面向对象中一个非常重要的特征。指的是：子类继承父类的属性和方法。 

### 1.继承的好处：

1. 子类拥有父类所有的属性和方法（代码复用）；
2. 子类可以扩展自己的属性和方法（更灵活）；
3. 子类可以重写父类的方法

### 2.继承方式

​	（一）原型链继承

- 核心：拿父类实例来充当子类原型对象，能使用原型链上的属性方法

- 缺点：

  - 无法继承构造函数中的属性
  - 创建子类实例时，无法向父类构造函数传参
  - 原型对象中存在多余的属性

  （二）借用构造函数

  ![1539306147160](C:\Users\覃珍珍\AppData\Local\Temp\1539306147160.png)

  核心：借父类的构造函数来增强子类实例，相当于把父类的实例属性复制一份给子类实例属性。

  call 用法 : 父类构造函数.call(子类实例,参数1,参数2,参数3...) 

  apply 用法 ：父类构造函数.apply(子类实例,[参数1,参数2,参数3...]) 

  【call与apply的唯一区别：传参方式不同，call多个参数，apply只有两个参数，第二个参数为数组 】

  ```
  
  ```

  缺点：

  ​	1.无法实现函数复用

  ​	2.函数太多就影响性能，占用更多内存

  （三） 组合继承  【原型链使用方法，借用构造函数拷贝属性】

  由于以上继承方法的缺点，实际开发中不可能单纯的只使用一种继承方法，而是利用它们的优点，规避它们的缺点，所以就有了组合继承法 。

  继承属性：借用构造函数

  继承方法：原型链继承

  组合继承：最常用的继承模式。

  ​	缺点（原型链继承法的缺点）：

  ​		在原型对象中生成多余的属性

  ​		多次执行父类构造函数

  （四）原型式继承

  ![1539306215869](C:\Users\覃珍珍\AppData\Local\Temp\1539306215869.png)

  核心：先创建了一个临时性的构造函数，然后将传入的对象作为这个构造函数的原型，最后返回了这个临时类型的一个新实例 【解决原型链继承法的缺点：生成多余的属性 】

  ```
  function object(o){
          function F(){}
          F.prototype = o;
          return new F();
      }
  ```

  - ES5版本的原型式继承：Object.create()

  （五）寄生组合继承法 【使用原型式使用方法+借用构造函数拷贝属性】

  完美的继承方法

  ​	核心：

  ​		继承属性【借用构造函数】   

  ​		继承方法【原型式继承】

  （六）ES6中的继承  【class】

  class

  ​	constuctor(参数){构造函数}

  ​	原型对象的方法：

  ​	init（）{}

  extends继承（原型式继承）  

  ​	class Dog extends Animal

  ​	super(属性)  拷贝父类构造函数的属性

  ​	constuctor(){super(属性)}  

  static静态方法  

  ​	【静态方法必须通过构造函数去调用，或者类去调用】

  

  

  ### Class定义类

  ES6提供了更接近传统语言的写法，引入了Class（类）这个概念，作为对象的模板。通过class关键字，可以定义类

  ```
  //定义类
  class Person {
      constructor(name,age) {
          this.name = name;
          this.age = age;
      }
      getInfo() {
           return `我叫${this.name},今年${this.age}岁`;;
      }
  }
  
  写在类里面的方法实际是给Person.prototype添加方法
  constructor方法是类的默认方法，通过new命令生成对象实例时，自动调用该方法。如果没有constructor方法，则得使用默认的constractor方法
  ```

  ### extends继承

  ```
  class Person {
      constructor(name, age) {
          this.name = name;
          this.age = age;
      }
  }
  
  class Man extends Person {
      constructor(name, age, gender) {
          //this.gender = gender; // 报错
          super(name, age);
          this.gender = gender; // 正确
      }
  }        
  ```

  - 子类继承了父类，在子类构造函数中必须调用super方法。
  - 子类的constructor方法没有调用super之前，不能使用this关键字，否则报错，而放在super方法之后就是正确的。

  （七）静态方法 【如果在一个方法前，加上static 关键字，否则报错，而放在super方法之后就是正确的。】

  ```
  class Person {
      constructor(){
          this.name = 'laoxie',
          this.age = 18;
      }
      static getInfo(){
          return this.name
      }
      say(){
          console.log(`Hello everyone, my name is ${this.name}, I'm ${this.age} years old`)
      }
  }
  class Man extends Person {}
  ```

  - 静态方法方法不会被实例继承，而是直接通过类来调用`Person.getInfo()`
  - 父类的静态方法，可以被子类继承`Man.getInfo()`

案例：1.扩展原生js的方法

​		兼容性字符串的trim方法

​		获取ASCII码的方法

​		反编译ascii码的方法



# svn版本管理工具







# 插件

插件的定义：插入进来的一些组件，不需要自己写。一般插件是一个对象，对象里面有多个函数【将实现某个功能的代码封装起来】

​	1.了解jQuery封装



![1539312183470](C:\Users\覃珍珍\AppData\Local\Temp\1539312183470.png)

判断是不是一个对象

如何用原型对象的方法？

原型式继承：可以使用原型链上的属性和方法

![1539324492185](C:\Users\覃珍珍\AppData\Local\Temp\1539324492185.png)



方法重写：.









