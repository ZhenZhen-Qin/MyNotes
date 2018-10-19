# 一、PHP环境安装

## 一、wampserver安装及使用

### 1. wampserver安装

- a:安装 Web 服务器Apache
- p:安装 PHP解析器
- m:安装MySQL数据库

> 对于初学者建议使用集成的服务器组件（如：WampServer），它已经包含了 PHP、Apache、Mysql 等服务,免去了开发人员将时间花费在繁琐的配置环境过程。

WampServer下载地址：<http://www.wampserver.com/>

若是缺乏某个dll，直接安装[vc_redist.x64_2015.exe](./anzhuangbao/vc_redist.x64_2015.exe)

【计算机-管理-服务和应用程序-服务-wampapache-wampmysql（开启服务前，先停止svn、mysql、iis）】

> 测试：
>
> 当wampserver图片变成绿色：
>
> 1. 网址中输入localhost或127.0.0.1（本机ip），默认打开C:\wamp64\www路径下的index.php
> 2. 同样可以localhost/*** 访问默认路径下（C:\wamp64\www）的任意文件

### 2.创建虚拟目录

- 左键小图标---->apache---->alias directories--->add an alias 
- 创建虚拟目录名字，例如aaa。enter后
- 创建虚拟目录对应的url，即需要开启web服务的文件夹路径(路径不可有中文、空格)
- 以上步骤完毕，访问虚拟目录只需在浏览器输入http://localhost/aaa，可访问目录下任意文件。

### 3.创建端口

- 左键小图标---->apache---->httpd-vhost.conf

- 复制如下代码，往下写。（//只是注释，记得删掉）

- ```
  listen 1704    //端口号
  <VirtualHost *:1704>      //端口号
  	ServerName localhost
  	DocumentRoot D:\laoxie\01Basic      //端口对应url(不可有中文路径、空格)
  	<Directory  "D:\laoxie\01Basic">    //端口对应url
  		Options +Indexes +Includes +FollowSymLinks +MultiViews
  		AllowOverride All
  		Require all granted     //让所有的主机都可以访问你的服务器
  	</Directory>
  </VirtualHost>
  ```

  以上步骤完毕，访问端口下目录只需在浏览器输入http://localhost:1704，可访问目录下任意文件。

### 4.开启局域网服务器

​	1.查看本机ip地址：common+r进入cmd，输入ipconfig，查看ipv4。

​	2.在vhost.conf给端口对应的url添加一句代码

```
#Require local //#代表注释
Require all granted    //允许其他主机访问
```

# 二、PHP内容

### 1.概念

> PHP是一种通用开源服务端脚本语言，将程序嵌入到HTML文档中去执行，结果以纯 HTML 形式返回给浏览器。		【HTML文件可嵌套PHP的代码，但是PHP文件不能嵌套HTML语句】
> PHP: Hypertext Preprocessor “超文本预处理器”，1994年由Rasmus Lerdorf创建，刚刚开始仅仅是为了要维护他本人个人网页而制作的一个简单程序（Perl语言编写），原名Personal Home Page（PHP由此得名），后用C语言重新编写，改名Hypertext Preprocessor

##### PHP能做什么：

- 生成动态页面内容
- 创建、打开、读取、写入文件
- 收集ajax数据
- 发送和接收cookie
- 添加、删除、修改您的数据库中的数据
- 限制用户访问您的网站上的一些页面
- 加密数据

### 2.基本语法

> - 默认文件扩展名是 “.php”。  【通常放在一个项目的api文件中】
> - 通常包含 HTML 标签和一些 PHP 脚本代码。

##### 分界标示符

```
    <?php //开始 

    //...php代码
  
    ?> //结束
```

##### 注释

> 与js一样，分单行和多行注释

- 单行注释：`//`
- 多行注释：`/**/`

##### 输出语句

- ##### echo

  只能输出一个或多个字符串（字符串可以包含HTML标签），速度较快，一般用于向前端返回数据

  ```
  <?php
      //输出一个字符
      echo "Hello world!<br>";
      //输出多个字符
      echo "This", " string", " was", " made", " with multiple parameters.";
  ?>
  ```

  ```
  json_encode(array,JSON_UNESCAPED_UNICODE);  把数组转成字符串
  	php5.4+ 使用JSON_UNESCAPED_UNICODE防止中文转义
  json_decode(json,assoc);把字符串转成数组/对象
  	json：待解码的 json string 格式的字符串
  	assoc：默认false,返回object, 当该参数为 true 时，将返回array
  ```

- ##### print_r()

  打印关于变量的信息，适用于数组、对象的打印，一般用于测试

- ##### var_dump()

  判断一个变量的类型与长度,并输出变量的数据类型和数值，一般用于测试

### 3.变量

##### 命名规则

- 以 $ 符号开始，后面跟着变量的名称（$称为标识符，不属于变量组成部分）
- 只能包含字母、数字字符以及下划线，不能以数字开头（不能包含空格）
- 区分大小写

```
//PHP 没有声明变量的命令，也没有声明提前的概念。
//变量在第一次赋值时被创建：
<?php
    $txt="Hello world!";
?>
```

##### 拼接字符串及变量

```
<?php
    $myname = "lemon";  
    echo 'My name is' . $myname;  //1.通过并置运算符.拼接字符串及变量
    //或者
    echo "My name is $myname"； //2.双引号内可以直接输出变量，无需拼接
?>
```

##### 函数中访问全局变量！！！

- 全局变量：在函数外部定义的变量，可以在任意位置访问(需要手动定义为全局变量)
- 局部变量：函数内部声明的变量，仅能在函数内部访问

（1） $GLOBALS

```
<?php
    $x='global x';
    function myTest(){
        //echo $x;//报错 Undefined variable:未声明定义的变量
        echo $GLOBALS['x']; //1.获取全部变量正确写法：$GLOBALS[变量名]，其中变量名不带$。

        //2.同时，可以在函数中创建全局变量
        $GLOBALS['y'] = $GLOBALS['x'] + $GLOBALS['y'];
    } 
    myTest();
    echo $y;
?>
```

（2）global 关键字	【写在别的域里，未出现过的域里】

```
    <?php
        $x=5;
        function myTest(){
            global $x;   //未出现过的域里
            $y=$x;
        }
        myTest();
        echo $y; 
    ?>
```

##### 超级全局变量

- `$GLOBALS`
  是PHP的一个包含所有全局变量的数组，可以在任意位置使用
- `$_SERVER`
  是一个包含了头信息(header)、路径(path)等信息的数组
- `$_POST / $_GET`
  被广泛应用于收集表单数据，常用于ajax请求等操作
- `$_COOKIE`
  用于收集前端发送过来的cookie数据
- `$_REQUEST`
  变量包含了 `$_GET、$_POST 和 $_COOKIE` 的内容
- `$_SESSION`
  服务器版cookie
- `$_FILES`

##### 常量

- 规范
  - 命名规则与变量一致，但常量名不需要加 $ 修饰符。
  - 常量值被定义后，在脚本的其他任何地方都不能被改变。
  - 默认是全局作用域，可以在整个运行的脚本的任何地方使用。
  - 常量名建议全部大写
- 格式： `define(name常量名称,value常量值)` 

```
define("EN_NAME", "laoxie");   // const MY_NAME = "lemon";
```

### 4.运算符及语句（等同于js）

​	算术运算符、赋值运算符、递增/递减运算符、比较运算符(等于大于...)、逻辑运算符(与或非)、三元运算符
	条件语句、循环语句

### 5.数据类型

$$
String（字符串）
Integer（整型）
Float（浮点型）
Boolean（布尔型）
Array（数组）
Object（对象）
NULL（空值）
$$

##### String

- strlen() 获取字符串长度，得到的字符的字节数

- mb_strlen() 获取字符串长度，

- strpos() 查找某个字符在字符串中的索引，如果未找到匹配，则返回 false

  ```
  strpos("Hello world!","world");//=>6
  ```

##### Array

> 数组是一个能在单个变量中存储多个值的特殊变量。

###### （1）创建数组：array()

```
//1.数值数组，等同于js的数组
$cars=array("Volvo","BMW","Toyota");
//2.关联数组，等同于js的对象
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
//3.多维数组，包含一个或多个数组的数组
```

###### （2）数组常用方法

- count() 获取数组长度
- in_array() 判断某个值是否存在数组中
- array_slice() 从数组中取出一段【array_slice(要取出的数组名，截取的开始索引，截取的数量[如果不写，默认到最后])     】
- array_rand() 随机获取数组的索引值

###### 练习案例：在php文件里生成动态商品页面

###### （3）遍历数组

- for	一般用于遍历数值数组

- foreach() 一般用于遍历关联数组,或者对象【关联数组必须要用foreach(),没有索引只有键值】

  ```
  <?php
      //遍历数值数组：for循环
      $cars=array("Volvo","BMW","Toyota");
      $arrlength=count($cars);
      for($x=0;$x<$arrlength;$x++){
          echo $cars[$x] . "<br>";
      }
      foreach($age as $item){
          //item代表数组里面每一项的值
      }
      //关联数组：
      $age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
      //获取单个值
      echo $age['peter'];
      //遍历关联数组：foreach..as
      foreach($age as $key=>$value){
          echo $key .":". $value;
          echo "<br>";
      }
  ?>
  ```

###### 数组排序

- sort() 对数组进行升序排列
- rsort() 对数组进行降序排列
- asort() 根据关联数组的值，对数组进行升序排列
- ksort() 根据关联数组的键，对数组进行升序排列
- arsort() 根据关联数组的值，对数组进行降序排列
- krsort() 根据关联数组的键，对数组进行降序排列









# 三、Ajax

## 了解AJAX

- AJAX: Asynchronous Javascript And Xml，Ajax 技术的核心是XMLHttpRequest对象（简称XHR），这是由微软首先引入的一个特性，其他浏览器提供商后来都提供了相同的实现

*Ajax起源：
最早出现在2005年的google搜索建议

- ajax优点
  - 增加速度：减轻服务器的负担,按需加载数据,最大程度的减少冗余请求
  - 改善的用户体验：局部刷新页面,减少用户等待时间,带来更好的用户体验
  - 页面和数据分离：前后端分离，操作更灵活，后期维护更方便
- 后端语言和服务器配置
  - php + Apache + mySQL
  - NodeJS + MongoDB
  - Java + tomcat + Oracle
  - .NET + IIS + SQL Server

## json

#### json数据(json字符串)

```
{"id" : 21465461461461, "name": "张三"},[{"id" : 21465461461461, "name": "张三"}]
```

#### json字符串与对象的转换

```
//（一）json字符串转成对象的转换
//1. eval("("+json字符串+")"); 
它的作用是，将一个普通的json格式的字符串，转换成Json格式的对象
//var list = eval("("+request.responseText+")");
//2. JSON.parse(); //把JSON字符串转成JSON对象(js对象/数组)【es5】

//（二）把JSON对象转成JSON字符串
JSON.stringify(); 
```

#### 了解json文件存在的意义

```
//模拟数据(与后端先商量)
[
	{
		"id":"G001",
		"name":"Thermos 膳魔师 Funtainer系列水杯 12盎司（340g） 粉红色",
		"imgurl":"images/g1.jpg",
		"price":899,
	}
]	
```

## Ajax请求步骤

#### 创建请求对象,返回一个异步请求对象

`var xhr = new XMLHttpRequest();`

#### 处理服务器返回数据

```
xhr.onreadystatechange = function(){
    if(xhr.readyState == 4) {
        //responseText：保存服务器返回的数据（从服务器返回的数据是“字符串”）。
        alert(xhr.responseText);
    }
}
```

#### 设置请求参数，建立与服务器连接

```
xhr.open("get", "http://localhost/api/ajaxtest", true);
```

#### 向服务器发送请求

`xhr.send(null);`

###### 案例：演示向goodslist.json请求数据

## XMLHttpRequest对象属性方法

#### open(type,url（同源策略）,async（同步、异步）)

get方法和post方法的区别：

```
1.open(type,url,async): 建立与服务器的连接
    type：请求的类型，get、post
    	* 区别? 
    		get请求数据接在url后面，post请求数据通过send方法传递
    		get传递的数据会比较少，post没有限制
    		get传递的数据会暴露出来
    url：数据请求的地址（API地址），一般由后端开发人员提供
        * 当前页面访问地址，API地址两者一定要同域
        * 同域（同源策略）：协议，域名，端口三者一致
        * 报错： No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'null' is therefore not allowed access.
	async：是否异步发送请求（true,false），默认为true
        * 同步：按步骤顺序执行，前面的代码执行完后，后面的代码才会执行
        	做完前一件事情, 才能下一件事情（排队）
        * 异步：与其他操作同时执行，也叫并发（图片加载，ajax请求，定时器）
```

#### send(data)

```
2.send(data): 向服务器发送请求
	data：可选参数，post请求时才生效，表示发请求时传送的数据字符串。
    	 xhr.send('size=20&type=music');
		在某些浏览器中，如果不需要通过post请求主体发送数据，则必须传入null
//备注：get请求的数据写在url地址后
	request.open("get", "http://localhost/api/getdata.php?type=get&qty=10", true);
	setRequestHeader(key,val)：设置请求头
//备注：利用请求头设置POST提交数据格式：
	xhr.setRequestHeader('content-type','application/x-www-form-urlencoded');//open方法后设置
```

> **在请求收到服务器的响应后，响应的数据会自动填充xhr 对象的属性，相关的属性简介如下：

#### readyState

```
0 － （未初始化）尚未调用open()方法。
1 － （启动）已经调用open()方法，但尚未调用send()方法。
2 － （发送）send()方法执行完成，但尚未接收到响应。
3 － （接收）已经接收到部分响应数据。
4 － （完成）响应内容解析完成，可以在客户端调用了
只要readyState 属性的值由一个值变成另一个值，都会触发一次readystatechange 事件。必须在调用open()之前指定onreadystatechange事件处理程序才能确保跨浏览器兼容性。
```

#### responseText

​	保存服务器返回的数据（从服务器返回的数据是“字符串”）。

#### status

```
* 响应的HTTP 状态。
200（OK）：服务器成功返回了页面
304（Not Modified）：数据与服务器相同，不需要重服务器请求（直接使用缓存的数据）
400（Bad Request）：语法错误导致服务器不识别
401（Unauthorized）：请求需要用户认证
404（Not found）：请求地址不存在
500（Internal Server Error）：服务器出错或无响应
503（ServiceUnavailable）：由于服务器过载或维护导致无法完
```

# 四 、php本地数据操作

#### 获取前端数据

```
$_GET 获取前端用get方式传递过来的数据（url地址后面数据也能被获取）
$_POST	获取前端用get方式传递过来的数据
isset() 判断某个值是否被设置，若不存在返回boolean
```

#### 文件的读取与写入

##### fopen(path,mode)：打开文件

> 使用fopen函数打开文件时，你首先需要明确打开文件的模式：
> 1.将数据写入文件，亦或者读写文件
> 2.考虑如果文件中已经存在相关数据，你是覆盖原有文件中的数据呢，还是仅仅将新数据添加至文件末尾

> 文件模式:
>
> - r 以只读方式打开文件，从文件头开始读
> - r+ 以读写方式打开文件，写入时以追加的方式写入文件
> - w 以写入方式打开文件，从文件头开始写。文件不存在则尝试创建，若存在，则先删除文件中的内容
> - w+ 以读写方式打开文件，从文件头开始读写。文件不存在则尝试创建，若存在，则先删除文件中的内容
> - a 以写入方式打开，从文件末尾开始追加写。如果文件不存在则尝试创建
> - a+ 以读写方式打开，从文件末尾开始追加写入或者读。如果文件不存在则尝试创建。

##### fread(file,length)：读取内容

##### fwrite(file,json字符串)：写入内容

##### fclose(file)：关闭文件,避免资源占用

##### filesize(path)：读取文件字符长度

```
//1.以读取模式打开文件
$path = './data/weibo.json'
$myfile = fopen($path, 'r');

//2.读取文件内容
$content = fread($myfile, filesize($path));

//3.关闭文件，减少资源占用
fclose($myfile);
```

```
echo返回数据
json_encode(); 把数组转成字符串
	php5.4+ 使用JSON_UNESCAPED_UNICODE防止中文转义
json_decode(json,assoc); 把字符串转成数组/对象
	json：待解码的 json string 格式的字符串
	assoc：默认false,返回嵌套对象的多维数组，通过arr->key获取对象键对应的值。当该参数为 true 时，将返回嵌套关联数组的多维数组，通过arr[key]获取关联数组键对应的值。
```

###### 案例：微博点赞

```
//（html页面）1.发起ajax请求，将weibo.json的内容返回到页面
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function(){
    if(xhr.readyState === 4 && xhr.status === 200){
        var res = JSON.parse(xhr.responseText);
        ul.innerHTML = res.map(item=>{
            return `<li data-id="${item.id}">
            <h4>${item.username}</h4>
            <p>${item.content}</p>
            <span>${item.comtcnt}</span>
            <span class="like">${item.likecnt}</span>
            </li>`;
    	}).join('');
    	datalist.appendChild(ul);
    }
}
xhr.open('get','../data/weibo.json',true);
xhr.send();
//(html页面)2.点赞，获取到当前按钮对用的id，发送ajax请求，get方法通过url拼接参数传送id到后台。
//随后php页面对json数据进行修改，再将当前被改变的对象返回。xhr对象通过responseText进行接收为字符串，转成对象，获取到linkcnt键对应的值，给当前事件源修改innerHTML。
//考虑多次请求，可能存在缓存状态，所以补充若是status为304也为请求成功。
datalist.onclick = function(e){
    if(e.target.className === 'like'){
        // e.target.innerText = e.target.innerText*1+1;
        // 获取当前微博id
        let currentLi = e.target.parentNode;
        let id = currentLi.dataset.id;
        let xhr = new XMLHttpRequest();
        let status = [200,304];
        xhr.onreadystatechange = function(){
            if(xhr.readyState === 4 && status.indexOf(xhr.status)>=0){
                let res = JSON.parse(xhr.responseText);
                e.target.innerHTML = res.likecnt;
            }
        }
        xhr.open('get','../api/weibo.php?id='+id,true);
        xhr.send();
    }
}
//（php）
//1.接收前端传过来的id，返回值为字符串
$id = isset($_GET['id']) ? $_GET['id'] : Null;
//2.打开json文件fopen()，读取json文件数据fread()。
$path = '';//相对路径
$file = fopen($path,"r");
$content = fread($file,filesize($path));
fclose($file);
//2.对得到的content内容转成json数组后，遍历里面每个item对象，若id键对应的值等于传入的$id,将该对象的linkcnt++。
$contentArr = json.decode($content);//此处contentArr为对象数组
$res;
foreach($contentArr as $item){
    //此处item为一个对象
    if($item->id == $id){
        $item->linkcnt++;
        $res = $item;
    } 
}
//4.将contentArr转成字符串，重新写入json文件
$file = fopen($path, 'w');
fwrite($file, json_encode($arr,JSON_UNESCAPED_UNICODE));
fclose($file);
//5.将步骤2声明变量res，将改变的item对象用res接收，转成字符串，echo给前端
echo json_encode($res,JSON_UNESCAPED_UNICODE);
```

###### 补充：eval的使用

```
var json = '{"name":"lemon","age":18}'; //标准json字符串
var json = '{"name":"lemon",\'age\':18}';//不标准
eval('('+json+')');//也可以转成对象
eval('1+2');//3
eval('定义函数；执行函数') //函数可以在字符串中执行
```

###### 讲解：ajax的来历及同源策略、同步异步

###### 案例：用户名验证

```
username.onblur = function(){
    let _username = username.value;
    let status = [200,304];
    let xhr = new XMLHttpRequest();
    //xhr.onload意思为数据请求完成后，等同于xhr.readystate==4的状态
    xhr.onload = function(){
        if(status.includes(xhr.status)){
            let res = xhr.responseText;//fail,success
            if(res === 'fail'){
                username.nextElementSibling.innerHTML = `${_username}太受欢迎，你走吧`；
            }else{
                username.nextElementSibling.innerHTML = `恭喜你绿了`；
            }
        }
    }
    xhr.open('get','../api/checkname.php?username='+_username,true);
    xhr.send();
}
//php
//1.数组存放已经存在的用户名
$userlist = array('张三','李四','王尼玛','奥巴马','laoxie','lemon');
//2. 获取前端传来的用户名
$username = isset($_GET['username']) ? $_GET['username'] : null;
//3. 判断前端传来的用户名是否已存在数组内
$res = in_array($username, $userlist);
if($res){
    // 用户名已存在，注册失败
    echo "fail";
}else{
    echo "success";
}
```

###### 案例：分页数据加载

```
//html页面：
var qty = 5;
// 1.发起ajax请求数据，拿到返回的数据，转成对象。
let status = [200,304]
let xhr = new XMLHttpRequest();
xhr.onload = function(){
    if(status.includes(xhr.status)){
        let res = JSON.parse(xhr.responseText);
        // 2.拿到对象的data键对应的值(为json字符串)，渲染页面
        datalist.innerHTML= res.data.map(function(item){
            return `<li>
            <h4>${item.name}</h4>
            <div>${item.content}</div>
            </li>`;
        }).join("");
        //3.得到总页码数pageNum，parseInt(数据总数res.total/每页数据qty).遍历生成页码
        var pageNum = parseInt(res.total/res.qty);
        page.innerHTML = "";
        for(var i=0;i<pageNum;i++){
            var span = document.createElement("span");
            span.innerHTML = i+1;
            //5.拿到返回的当前页码-1，把当前索引设置高亮
            var curidx = res.curpage -1;
            if(i == curidx){span.className = "active";}
            page.appendChild(span);
        }

    }
}
xhr.open('get','../api/lemon.php?qty='+qty+'&curpage=1',true);
xhr.send();
// 4.点击分页切换
page.onclick = function(e){
    if(e.target.tagName.toLowerCase() === 'span'){
        let curpage = e.target.innerText;
        xhr.open('get','../api/lemon.php?qty='+qty+'&curpage='+curpage,true);
        xhr.send();
    }
}
//php：
<?php
    $qty = isset($_GET["qty"])? $_GET["qty"] : 5;
    $curpage = isset($_GET["curpage"])? $_GET["curpage"] : 1;
    //1.拿到json数据,根据每页数量与当前页，裁切对应的数据
    $path = '../data/football.json';
    $file = fopen($path,'r');
    $content = fread($file,filesize($path));
    $data = json_decode($content);
    $curdate = array_slice($data,$qty*($curpage-1),$qty);
    //2.格式化数据，再返回给前端
    $res = array(
        "total" => count($data),
        "data" => $curdate,
        "qty" => $qty*1,
        "curpage" => $curpage*1,
    );
    echo json_encode($res,JSON_UNESCAPED_UNICODE);
?>
```



## 五、ajax跨域解决方案

### JSONP

- JSONP 是JSON with padding（填充式JSON 或参数式JSON）的简写。
- JSONP是一种可以绕过浏览器的安全限制，从不同的域请求数据的方法。
- JSONP请求不是ajax请求，是利用script标签能加载其他域名的js文件的原理，来实现跨域数据的请求 
- 局限性：
  - 只能为get请求
  - 接口必须有回调函数的执行

###### 演示：使用script标签其他js文件调用本地js的某个函数

```
//html页面：
<script type="text/javascript">
    function sum(n){
        console.log(n);
    }
</script>
<script type="text/javascript" src="../js/common.js"></script>

//common.js代码
sum(6666);

//此时html页面的控制台打印 6666。
```

###### 演示：使用script标签其他php文件调用本地js的未知名方法，返回数据

```
//html页面：
<script type="text/javascript">
    function sum(data){
        console.log(data);
    }
    function sum2(data){
        alert(data);
    }
</script>
<script src="../api/lemon.php?callback=函数名"></script>

//php文件：
<?php
    $fn = $_GET["callback"];
    echo "$fn(数据)";
?>
```

###### 案例：利用JSONP原理调用百度建议

```
msg.oninput = function(){
    let _msg = msg.value;
    clearTimeout(timer);
    timer = setTimeout(()=>{
    let script = document.createElement('script');
    script.src='https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/sujson=1&cb=getData&wd='+_msg;
    document.body.appendChild(script);
    },500);
}
window.getData = function(data){
    suggest.innerHTML = data.s.map(item=>{
        return `<li>${item}</li>`
        }).join("");
    }
})
//原理性代码：
//1.script的src中回调函数的传递
script.src='https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/sujson=1&cb=getData&wd='+_msg;
//2.声明全局函数
window.getData = function(data){处理数据}
```

### CORS

CORS是一个W3C标准，全称是”跨域资源共享”（Cross-origin resource sharing），它允许浏览器向跨源服务器发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制。

CORS需要浏览器和服务器同时支持。目前，所有浏览器都支持该功能，IE浏览器不能低于IE10。

局限性：服务器必须设置响应头，浏览器的兼容问题

（同样的发起ajax请求）

```
1.Access-Control-Allow-Origin
	header('Access-Control-Allow-Origin: * ');  //设置跨域请求必须要加上
该字段是必须的。需要在后端响应头添加词字段，值要么是一个*，表示接受任意域名的请求，要么指定一个域名http://localhost。
2.Access-Control-Allow-Methods
3.Access-Control-Allow-Headers
    header('Access-Control-Allow-Methods:POST');  
    header('Access-Control-Allow-Headers:x-requested-with,content-type'); 
```

###### 案例：天气预报

```
//1.直接利用ajax请求得到数据
let xhr = new XMLHttpRequest();
let status = [200,304]
xhr.onload = function(){
    if(status.includes(xhr.status)){
        let res = JSON.parse(xhr.responseText)
        console.log(res.data);
        //拿到数据，可以渲染部分到页面上
        let title = document.createElement('h2');
        title.innerHTML = res.data.city + '天气预报';
        let tips = document.createElement('p');
        tips.innerHTML = res.data.ganmao;
        let ul = document.createElement('ul');
        ul.innerHTML = res.data.forecast.map(item=>{
            return `<li>
                <h4>${item.date}</h4>
                <p>${item.low} / ${item.high}</p>
                <p class="type">${item.type}</p>
            </li>`
        }).join('\n');
        // 清空内容
        weather.innerHTML = '';
        weather.appendChild(title);
        weather.appendChild(tips);
        weather.appendChild(ul);
    }
}
xhr.open('get','http://wthrcdn.etouch.cn/weather_mini?city=广州',true);
xhr.send();
//2.配合文本框实现，得到不同城市的天气预报
city.onblur = function(){
    let _city = city.value.trim();
    if(_city.length === 0){
        return;
    }
    xhr.open('get','http://wthrcdn.etouch.cn/weather_mini?city='+_city,true);
    xhr.send();
}
```

###### 演示：百度地图

### 服务器代理

后端不存在跨域问题，所以可以利用后端间接获取其他网站的数据

```
ajax跨域请求之服务端代理（爬虫）
原理：获取页面所有内容，并利用正则匹配所需内容
file_get_contents($url) //获取网站内容
preg_match_all($reg,$str,$res) 
preg_match($reg,$str,$res)   //$str代表被匹配的内容，$res为结果
$content = iconv(原字符编码,新字符编码,$content);//修改$content字符编码
```

###### 案例：利用服务器代理获取外网IP

```
api地址：http://2018.ip138.com/ic.asp
//本地api代理
<?php
    $content = file_get_contents("http://2018.ip138.com/ic.asp");//拿到远程网站内容
    $content = iconv('gb2312','utf-8',$content);//修改网站内容字符编码与相应头一致
    preg_match_all('/\[([\d\.]+)\]/',$content,$res);//正则匹配
    echo $res[1][0];
?>
//前端访问本地api
```

###### 案例：根据IP获取所在城市（ajax嵌套）

```
api地址：http://ip.taobao.com/service/getIpInfo.php?ip=$ip
//利用上一个案例请求拿到的ip，传入本地api。
let xhr_city = new XMLHttpRequest();
xhr_city.onload = function(){
    if(status.includes(xhr_city.status)){
        let city = xhr_city.responseText;
        //返回的是一串html结构加城市名，通过正则拿到城市名
        city = city.match(/[\u2E80-\u9FFF]+/)[0];
        console.log(city);
    }
}
xhr_city.open('get','../api/lemon.php?ip='+ip,true);
xhr_city.send(null);
//本地api
$ip = isset($_get['ip']) ? $_get['ip'] : "27.46.236.176";
$content = file_get_contents("http://ip.taobao.com/service/getIpInfo.php?ip=$ip");
$res = json_decode($content);
$data = $res->data;
$city = $data->city;
echo $city;
```

###### 案例：根据城市获取天气预报（ajax嵌套）

- 所有的代码都写在一个function里面，数据混乱，容易出错。
- 维护困难

###### 演示：post请求数据

## Promise

Promise是一个构造函数，所谓的Promise对象，就是通过new Promise()实例化得到的对象，用来传递异步操作的消息。它代表了某个未来才会知道结果的事件（通常是一个异步操作），并且这个事件提供统一的 API，可供进一步处理。

##### Promise 的三种状态

> Pending（未完成）可以理解为Promise对象实例new创建时候的初始状态
> Resolved（成功） 可以理解为成功的状态
> Rejected（失败） 可以理解为失败的状态

#### 静态方法

##### Promise.all([p1,p2,p3...])

- 将多个Promise实例，包装成一个新的Promise实例

- 所有参数中的promise状态都为resolved时，新的promise状态才为resolved

- 只要p1、p2、p3..之中有一个被rejected，新的promise的状态就变成rejected 

  ```
  Promise.all([p1,p2,p3]).then(function(res){
  	console.log(res);
  })
  ```

##### Promise.race([p1,p2,p3...]) // 竞速，完成一个即可

#### 原型方法

##### Promise.prototype.then(successFn[,failFn])

> Promise实例生成以后，可以用then方法分别指定Resolved状态和Rejected状态的回调函数。并根据Promise对象的状态来确定执行的操作:
> resolved时执行第一个函数successFn
> rejected时执行第二个函数failFn。

##### Promise.prototype.catch(failFn)

###### 演示：定时器结束后再执行某些操作

```
var p = new Promise(function(resolve, reject){ 
	//做一些异步操作 
	setTimeout(function(){ 
		console.log('执行完成'); 
		resolve('随便什么数据'); 
	}, 2000); }
); 
p.then(function(data){
    //data为随便什么数据
}) 
```

###### 演示：生成一个0-2之间的随机数，如果小于1，则等待一段时间后返回成功，否则返回失败：

```
function test(resolve, reject) {
    var timeOut = Math.random() * 2;
    console.log(timeOut);
    setTimeout(function () {
        if (timeOut < 1) {
            resolve('200 OK');
        }
        else {
            reject('timeout in ' + timeOut + ' seconds.');
        }
    }, 1000);
}
var p1 = new Promise(test);
var p2 = p1.then(function (result) {
    console.log('成功：' + result);
});
var p3 = p2.catch(function (reason) {
    console.log('失败：' + reason);
});
```

###### 演示：利用promise完善ajax嵌套

> 有了Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数 

###### 加载图片，获取图片信息（宽高）

```
var p = new Promise(function (resolve, reject) {
    var image = new Image();//生成img标签
    image.src = 'http://image.baidu.com/xxx.jpg;
    image.onload  = ()=>{
        resolve(image);
    }
    image.onerror = reject;
  });
//使用
p1.then((img)=>{
    document.body.appendChild(img);
    console.log(img.offsetWidth,img.offsetHeight);
}) 
```

## try...catch

尝试执行代码，如果有错误则执行catch捕获错误(不报错)

```
try{
	console.log(666)
	//先尝试执行这里的代码
	show();
	//无报错，则忽略catch
	//如果报错，则执行catch，并传递错误信息
}catch(error){
	console.log(error)
}
console.log('end');
```

###### 演示：xhr的兼容写法

```
var req = null;
try{
    req = new XMLHttpRequest();
}catch(err){
    // ie低版本浏览有多种异步请求的实现
    try{
        req = new ActiveXObject("Msxml2.XMLHTTP");
    }catch(err){
        try{
            req = new ActiveXObject("Microsoft.XMLHTTP");
        }catch(err){
            alert('你的浏览器太low了，这个世界不适合你');
        }
    }
}
```



# 六、三层架构

### 1.概念

​	将整个业务应用划分为 ：界面层（User Interface layer）、业务逻辑层（Business Logic Layer）、数据访问层（Data access layer）。

​	B层是通过后台语言（例如php）处理业务逻辑，必须建立在某个web服务（例如apache、iis）的基础上，才能通过浏览器进行访问。

​	D层是对数据库的操作 ，同样也必须建立在某个服务的基础上，才能保证别人能访问到数据库。

### 3. wampserver安装

- a:安装 Web 服务器Apache
- p:安装 PHP解析器
- m:安装MySQL数据库

> 对于初学者建议使用集成的服务器组件（如：WampServer），它已经包含了 PHP、Apache、Mysql 等服务,免去了开发人员将时间花费在繁琐的配置环境过程。

WampServer下载地址：<http://www.wampserver.com/>

若是缺乏某个dll，直接安装[vc_redist.x64_2015.exe](./anzhuangbao/vc_redist.x64_2015.exe)

### 4.iis+php+mysql

#### 开启iis服务

控制面板\程序和功能\windows功能\internet信息服务所有选项都勾选上

验证：

​	控制面板/管理中心\iis manage（快捷方式拖拽至桌面）

​	开启服务-浏览器输入localhost:80-页面出现iis即成功

#### 添加php-manage

​	安装 PHPManagerForIIS-1.2.0-x64.msi 及 php-7.0.13-nts-Win32-VC14-x64 .

### ![php-cgi](C:/HTML5-1808/php/doc/images/php-cgi.PNG)

验证：

​	上图第一个圆圈，后面的选项check phpinfo（）

​	运行出现正常网页效果即可。

#### mysql

​	安装[mariadb](./anzhuangbao/mariadb-10.0.14-winx64.msi)

### 5.注意事项

​	iis服务与apache服务二选一，一方停止另一方才能开启。

​	mysql服务也是一样，不能同时开启多个。

#### 如何关闭、开启服务：

​	计算机/管理/服务和应用程序/服务！！！





# 六、php与mysql交互

PHP 5  及以上版本建议使用以下方式连接	mysql数据库

MySQLi extension (“i” 意为 improved) 安装：Linux 和 Windows: 在 php5 mysql 包安装时 MySQLi 扩展多数情况下是自动安装 



```
 <?php
        $servername = "localhost";
        $username = "username";
        $password = "password";
        $dbname = 'user';

        // 创建连接
        $conn = new mysqli($servername, $username, $password, $dbname);

        // 检测连接
        if ($conn->connect_error) {
            die("连接失败: " . $conn->connect_error);
        } 

        //查询前设置编码，防止输出乱码
        $conn->set_charset('utf8');

        echo "连接成功";
    ?>
```

1. 执行sql语句查询结果集

   ```
   //编写sql语句
       $sql = 'select * from student';
   
       //获取查询结果集
       $result = $conn->query($sql);
   
       //使用查询结果集
       //得到数组
       $row = $result->fetch_all(MYSQLI_ASSOC);
   
       //释放查询结果集，避免资源浪费   只有查询才需要关闭查询结果集
       $result->close();
   
       //把结果输出到前台
       echo json_encode($row);
   
       // 关闭数据库，避免资源浪费
       $conn->close();
   
   ```

   

2. 插入数据  insert into 表名（字段名）values(  要插入的记录)

   ```
   单条数据
   
   $sql = "INSERT INTO MyGuests (firstname, lastname, email)
   VALUES ('John', 'Doe', 'john@example.com')";
   
   if ($conn->query($sql)) {
       echo "新记录插入成功";
   } else {
       echo "Error: " . $sql . "<br>" . $conn->error;
   }
   
   多条数据
   
   $sql = "INSERT INTO `projects` (`name`, `url`, `description`) VALUES ";
       foreach ($projects as $item) {
           $sql .= "('$item',NULL, NULL),";
       }
       $sql = substr($sql,0,-1);
   
       if ($conn->query($sql)) {
           echo "新记录插入成功";
       } else {
           echo "Error: " . $sql . "<br>" . $conn->error;
       }
   ```

   

3. 查询数据（读取数据）     

   SELECT…FROM，得到查询结果集

   - num_rows ：结果集中的数量，用于判断是否查询到结果

   - fetch_all(MYSQLI_ASSOC) 得到所有结果

   - fetch_assoc() 得到第一个结果

   - fetch_row()

     ```
     $sql = "SELECT id, firstname, lastname FROM MyGuests";
         $result = $conn->query($sql);
     
         if ($result->num_rows > 0) {
             // 输出每行数据
             while($row = $result->fetch_assoc()) {
                 echo "<br> id: ". $row["id"]. " - Name: ". $row["firstname"]. " " . $row["lastname"];
             }
         } else {
             echo "0 个结果";
         }
     ```

     ​                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   

4. mysql 语句的返回值

   ```
   返回布尔值
   	insert
   	update
   	delete
   返回查询结果集
   	select语句
   ```





## 面向对象



1. 创建对象

   1. 字面量   var obj = {};   【一般用于单个对象】

   2. 构造函数  new Object()    【一般用于单个对象】

   3. 工厂函数  【产生的对象的构造函数都是Object，类不明确】

      ![1539050873066](C:\Users\覃珍珍\AppData\Local\Temp\1539050873066.png)

      1. ```
         //特点：生产出来的对象都是object类型
         ```

   4. 自定义构造函数（类）  new  自定义构造函数

      1. 解决了对象识别的问题

      2. 不成文的规定：为了区别书写构造函数与普通函数，构造函数首字母大写

      3. 构造函数与普通函数没有本质区别，唯一的区别是执行方式【new 函数（）  ===>   构造函数  |||    函数（）   ==>  普通函数，任何函数直接执行，没有return ， 返回值都是undefined】

         1. new实例化对象的过程，相当与构造i函数内部执行了四大步骤：
            1. 创建一个新对象

         ![1539051907516](C:\Users\覃珍珍\AppData\Local\Temp\1539051907516.png)

      4. ![1539133507930](C:\Users\覃珍珍\AppData\Local\Temp\1539133507930.png)

   5. 

2. 对象的操作

   1. 创建对象
   2. 描述对象（键值对  “.”  or  "[]"）
      1. 属性：描述对象有什么
      2. 行为：描述对象能做什么

3. 指挥对象

   1. 行为执行的时候要加括号

   

   

   构造函数：用new函数名（）  创建的对象，该函数就叫做构造函数

   实例对象：用new函数名（）生成的对象称为实例对象 【】![1539067140157](C:\Users\覃珍珍\AppData\Local\Temp\1539067140157.png)

   原型对象（prototype）：

   ​	实例对象的__proto__指向原型对象；

   ​	原型对象的constructor(构造器)指向构造函数；

   ​	构造函数的prototype指向原型对象。

   #### 自定义构造函数+原型对象:

   备注：实例对象能使用原型对象的属性和方法，而不是拥有。实例对象拥有构造函数的属性和方法。

   重点：创建对象时，若将所有的属性集方法都写在构造函数。会造成每个实例对象都拥有构造函数的所有的属性和方法，会造成内存的浪费。解决：私有属性写在构造函数；公共属性和方法写在原型对象。

   ![1539068790201](C:\Users\覃珍珍\AppData\Local\Temp\1539068790201.png)

   原型链：

   ![1539067456890](C:\Users\覃珍珍\AppData\Local\Temp\1539067456890.png)

   

   值属性的属性特性：

   ![1539151533480](C:\Users\覃珍珍\AppData\Local\Temp\1539151533480.png)

   1.利用点或者[]操作对象的属性方式，默认以上的属性特征都为true（除了value为具体的值）

   2.操作属性特性及属性的的方法

   ![1539151755331](C:\Users\覃珍珍\AppData\Local\Temp\1539151755331.png)

   













































