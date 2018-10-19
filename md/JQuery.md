JQuery

![1539567683824](C:\Users\覃珍珍\AppData\Local\Temp\1539567683824.png)





上下文对象：

​	jQuery(".aa","#output");   在ouput下的aa ,"#output"可以是选择器，也可以是jQuery对象，不成文的规定：jQuery接收jQuery对象的变量名，尽量用$开头，方便自己去使用

![1539568069393](C:\Users\覃珍珍\AppData\Local\Temp\1539568069393.png)

表单选择器：【常用】

​	在dom加载完之后执行里面的代码：jQuery（function（$）{

​	

​	$('input[type="radio"]');

​	表单选择器写法【这四个】：

​	:radio //匹配所有单选按钮
	:checkbox //匹配所有复选按钮

​	:selected //获取已选择的option元素

​	:checked //匹配所有被选中的元素(复选框、单选框等，select中的option)

}）

可见性选择器：

​	：hidden  匹配到所有不可见的元素及type为hidden的元素

​	：visible  可见的元素

​	EX：

​		var  $sel = $(":visible","#output");   匹配output的所有可见的元素

is()方法通常用于判断选择器是否存在，返回布尔值

常用操作：jQuery对象转原生DOM节点

​	1.？？？？方式1

​	1.[索引值]

​	2.get(0) 获取到第一个节点

​	3.get()  不传参数的话获取到所有的dom节点



判断一个是不是jQuery对象：长度大于0就是存在

![1539569629198](C:\Users\覃珍珍\AppData\Local\Temp\1539569629198.png)

jQuery对象.length == ?

EX： var  res = $(".aa").get(0)  获取到第一aaDOM节点



【console.log只能打印window变量】







筛选选择器：

1. :odd/even  ,   :gt(n)/lt(n)    筛选范围(索引从0开始)

```
jQuery（funcrtion($){
	$("ul li:odd").css("background","red");
}）
```

​	2.	:contains( 内容)  筛选出包含某内容的元素

筛选方法：![1539570805574](C:\Users\覃珍珍\AppData\Local\Temp\1539570805574.png)

1. 查找

   1. 查找子元素   children(css选择器)  获取到所有满足条件的选择器
   2. find（）   获取到所有满足条件的后代元素

   

   1. filter 和  not  是过滤自己
   2. has()  过滤用有满足条件的子元素的父类选择器
   3.  





dom节点无法与jquery混着使用：

​	dom节点的属性通过点或者[]  操作；get/setAttribute()

​	

​	jquery  ：  prop(attr[,value])   一个值表示获取     attr(,value)；

![1539585117056](C:\Users\覃珍珍\AppData\Local\Temp\1539585117056.png)

把dom节点转成jQuery对象：直接加$() 

判断是否含有这个类名  .hasClass（“”）

![1539586310066](C:\Users\覃珍珍\AppData\Local\Temp\1539586310066.png)







![1539651859625](C:\Users\覃珍珍\AppData\Local\Temp\1539651859625.png)



![1539652861022](C:\Users\覃珍珍\AppData\Local\Temp\1539652861022.png)



### Dom节点的操作：	

```javascript
创建dom节点
1.  $()
jQuery(function($){
    $("<div/>");
     $("<div>拉拉啦</div>");
});

插入子元素
内部插入：
    $box.append($("<div></div>"));作为父元素的最后一个的子元素插入
	prepend()  作为父元素的第一个子元素插入
		appendTo()     prependTo()
外部插入（添加为兄弟元素）
	after()在元素的前面添加
    before()在元素的后面添加
    	insertAfter() , insertBefore()

移除元素
    remove()  删除元素，自己删除自己
	empty()  清空元素内部，不包括自己
元素的复制
	clone()
		even:  (true 或 false)  是否复制元素的行为，默认false
        ...
    
    
    
    
    
```



元素盒模型属性

​	原生js  :  offsetWidth = content +padding + border

​			offsetTop/Left = 获取距离最近的定位父级的距离

​			clientWidth = content +padding 【可视区域的宽高】

​			scrollLeft/Top  【元素滚动条滚动的距离】



​	jQuery的写法：

![1539655804416](C:\Users\覃珍珍\AppData\Local\Temp\1539655804416.png)

1. width(v)    -->  content  存在参数v值时代表给width复制
2. innerWidth()   -->  content+padding
3. outerWidth(true) 			 
4. outerWidth()
5. ​                                                                                                                 

​		.outerWidth();    --->     offsetWidth \









事件的绑定：

​	1.on(type,[selector],fn)

​		selector把本来绑定给selector的事件委托给它的父级

​		可以给同一个元素绑定多个同一事件

​		事件命名空间：对事件进行细分，用法：事件类型.自定名字       Ex:   click.自定义名字

​		自定义事件：type的名字是自定 【执行：trigger(自定义类型)方法去手动执行事件】  trigger（）方法能执行所有的事件

​			triggerHandler(type)  不允许事件冒泡，同时不允许默认行为

​			 EX：![1539674232337](C:\Users\覃珍珍\AppData\Local\Temp\1539674232337.png)

​	2.off()

​		没有参数：移除该元素的所有的事件

​		参数为类型，移除该元素的该类型的所有事件

​	 

ajax方法：

​	$ajax({

​		url:   ,

type:   ,

data:{  },success : function(){

​	}

})



![1539675371407](C:\Users\覃珍珍\AppData\Local\Temp\1539675371407.png)







































