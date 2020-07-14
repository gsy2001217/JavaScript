# 红宝书第八章：BOM

## 	1：Window对象

​			BOM的核心就是Window，表示的是一个浏览器的实例。Window对象既是JavaScript访问浏览器的一个接口也是ECMAScript规定的Global对象。

### 		1.1：全局作用域

​		由于Window在ECMAScript扮演Global对象的角色，所以在全局声明的函数和变量都会变成Window的属性和方法；但是二者也是有区别的，直接在全局定义的变量不能够通过delete删除，但是直接在window对象上面定义的属性是可以的。例如

```javascript
var age=20;
window.color="red";
delete window.age;//返回false
delete window.color;//返回true
alert(window.age);//20
alert(window.color);//undefidend,因为在上面已经被删除了


/**
在var定义的语句添加了window属性有一个是[[Configurable]]特性，默认值是false，所以不可以被delete
*/
```

在访问未声明变量的时候回报错，但是可以通过查询window对象来知道某个为止变量是否存在

```javascript
var news=olds;//报错，因为olds不存在
var news=window.olds;//不报错，因为这是一次属性查询
```

### 1.2：窗口关系及框架

​		**top对象**始终执行最高层的框架，也就是浏览器窗口，我们可以保证正确的访问到另一个框架，但是对于在一个框架写的代码来说，window对象指向的都是那个框架的特定实例而不是最高层的框架

​		**parent对象**始终执行当前框架的直接上层框架。但是有时候parent对象也和top对象相等。注意：除非最高层窗口是通过window.open（）打开的，否则window的name属性是包含任何值的。

​		**self对象**他始终指向window，实际上，self与window对象是可以轮换使用的引入。

以上所有window对象的属性都可以通过window.parent 和window.top访问。

### **1.3：窗口位置**

​		确定是修改window对象位置的方法：1：screenLeft和screenTop来确定屏幕的左边和上边；2：screenX和screenY属性。可以使用如下的方法来判断浏览器支持的是哪种方法

```javascript
var leftPos =(typeof window.screenLeft=="number")?
    window.screenLeft:window.screenX;

var topPos =(typeof window.screenTop=="number")?
    window.screenTop:window.screenY;
```

### 1.4：窗口大小

​		四个属性：innerWidth，innerHeight，outerWidth和outerHeight。在IE9+,Sarfari和Firfox中后面两个返回的是浏览器窗口本身的尺寸，在Opera中前面两个则是值该容器页面视图区的对象（减去边宽度），但是在Chorme中都返回视口大小。

​		在IE，Firefox，Safari，Opera，Chorme 中document.documentElement.clientWidth和document.documentElement.clientHeight保存了页面视口的信息

### 1.5：导航和打开窗口

​		使用window.open()既可以导航到一个特定的URL也可以打开一个新的浏览器窗口。接收四个参数：URl，窗口目标，一个特性字符串，以及一个表示新页面是否取得浏览器历史记录中当前加成页面的布尔值。如果已经有了第二个参数，并且第二个参数是已有窗口或者框架的名称，，那么就会在具有改名称的窗口或者框架加载第一个参数指定的URL。**也可以是任意一个特殊的窗口名称：_self， _parent，_top，_blank**

​		1弹出窗口：

​		如果第二个参数不是一个存在的窗口或者框架，该方法会根据第三个参数传入的字符串来参加一个新窗口或者新标签页，如果没有第三个就会打卡一个带有全部默认设置的新浏览器窗口。下面是第三个窗口的设置选项

​	

| 设置       | 值     | 说明                                     |
| ---------- | ------ | ---------------------------------------- |
| fullscreen | yes/no | 浏览器是否最大化                         |
| height     | 数值   | 新窗口的高度不能小于100                  |
| left       | 数值   | 新窗口的左坐标，不能为负值               |
| location   | yes/no | 是否在浏览器窗口显示地址栏               |
| menubar    | yes/no | 是否在浏览器窗口显示菜单栏               |
| resizable  | yes/no | 是否可用通过拖动浏览器窗口的边框改变大小 |
| scrollbars | yes/no | 如果内容在视口显示不下，是否允许滚动     |
| status     | yes/no | 是否在浏览器窗口显示状态栏               |
| toolbar    | yes/no | 是否在浏览器窗口显示工具栏               |
| top        | 数值   | 新窗口的上坐标                           |
| width      | 数值   | 新窗口的宽度，不能小于100                |

​		2：安全限制

为了安全考虑，大部分浏览器会进制弹出窗口

​		3：弹出窗口屏蔽程序

使用下面的代码在任何时候都会检测出调用window.open打卡的弹出窗口是不是被屏蔽了。

```javascript
var blocked=false;
try{
    var wxo=window.open("xxxxx","_blank");
}catch(ex){
    blocked=true;
}

if(blocked){
    alert("The popup was blocked")
}
```

### 1.6：间隙调用和超时调用

​		超时调用是在指定的时间后执行代码，间隙调用是指每隔一段时间就执行一次代码

超时调用需要使用window对象的setTimeout方法，接收两个参数，第一个是包含JavaScript代码的字符串，也可以是函数；第二个参数是一个表示等待多长时间的毫秒数。在调用这个方法后返回一个数值ID，可以通过它来取消超时调用，这个时候需要用这个clearTimeout方法。

```javascript
//设置超时调用
var timeid=setTimeout(function(){
    alert("hello");
},100);
//取消超时调用
clearTimeout(timeid);

```

设置间隙调用的方法是setInterval方法，接收的参数与超时调用一样，这个方法一样会返回一个参数ID，可以使用clearInterval方法来取消间隙调用

```javascript
var num=0;;
var max=10;
var interid=null;
function interNum(){
    num++;
    if(num==max){
        clearInterval(interid);
        alert("yes");
    }
}
interid=setInterval(interNum,100);
```

### 1.7：系统对话框

​	在浏览器可以使用alert，confirm，prompt方法来调用系统对话框此昂用户显示信息。

​		一般来说，**alert**生成的“警告”对话框向用户显示一些无法控制的信息，比如错误信息；confirm方法生成的对话框有两个按钮，一个表示yes一个表示no，分别返回true和false；第三种是prompt方法，这是一个提示框，用于用户输出内容的，也会有一个表示yes一个表示no的按钮，如果是点击yes的话会返回文本框的内容，否则返回null。

```javascript
if(confirm("Are you sure")){
    alert("I'm so glad");
}else{
    alert("I'm sorry");
}

var result=prompt("What is your name","");
if(result!==null){
    alert("Welcome"+result);
}
```

## 2：location对象

t他提供了与当前窗口加载的文档有关的信息。

| 属性名   | 例子                 | 说明                                                      |
| -------- | -------------------- | --------------------------------------------------------- |
| hash     | “#contents”          | 返回URL的hash(#后面跟0或多个字符）                        |
| host     | “www.wrox.com:80”    | 返回服务器名称和端口号                                    |
| hostname | “www.wrox.com”       | 返回不带端口号的服务器名称                                |
| href     | “http:/www.wrox.com" | 返回当前加载页面的完整url。location的toString也是返回这个 |
| pathname | ”WileyCDA“           | 返回URL的目录或者文件名                                   |
| port     | ”8080“               | 返回端口号，如果URL没有就返回空字符串                     |
| protocol | ”http:"              | 返回页面使用的协议                                        |
| search   | “？q=J、javascript”  | 返回URL的查询字符串，以问号开头                           |



###  2.1：查询字符串参数

可以这样操作来解析字符串并且返回包含所有参数的一个对象

```javascript
function getQury(){
    //取得查询字符并去掉开头的问号
    var qs=(locatiion.search.length>0?location.search.substring(1):"");
    arg={};//保存数据的对象
    
    //取得每一项
    items=qs.length?qs.split("&"):[],
        item=mull,
        name=null,
        vaule=null,
        i=0,
        len=items.length;
    for(i=0;i<len;i++){
        item=items[i].split("=");
        name=decodeURLComponent(item[0]);
        value=decodeURLComponent(item[1]);
        if(name.length){
            args[name]=value;
        }
    }
    return args;
}
```

### 2.2位置操作

​		最常用的改变浏览器位置的方法是：location.assign，为其传递一个URL。也可以这样使用，

window.location=“xxxx”；或者location.href=“xxx”；效果一样。这个方跳转之后可以点击后退按钮回到前一个页面，如果要禁止的话可以使用replace来代替。使用reload方法是对页面进行重新加载，如果不加任何参数的话会从浏览器的缓存中加载，如果要从服务器加载需要在里面加上参数true。

## 3：navigator对象

可以使用plugins数组来实现检测是否按照了特定的插件，每个项都包含一下属性。

​	name：插件的名字，

​	description：插件的描述

​	filename：插件的文件名

​	length：插件所处理的MIME类型数量

```javascript
function hasPlugin(name){
    name=name.toLowerCase();
    for(var i=0;i<navigator.plugins.length;i++){
        if(navigator.plugins[i].name.toLowerCase().length.indexOf(name)>-1){
            return true;
        }
    }
    return false;
}

//检测flash
alert(hasPlugin("Flash"));
//检测QuickTime
alert(hasPlugin("QuickTime"));
```

### 3.2:注册处理程序

​		registerContentHandler和registerProtocolHandler方法可以让一个战队指明它可以处理特定类型的信息。

​		registerContentHandler接收三个参数，要处理的MIME类型，可以处理的MIME类型的页面的URL，以及应用程序的名称。

​		registerProtocolHandler接收三个参数，要处理的协议，处理该协议的页面的URL和应用程序的名称。

## 4：history对象

接收一个参数，表示向前或者向后跳转的页数，或者表示跳转到最近的某个指定的页面。

```javascript
history.go(-1);//后退一页
history.go(1);//前进一页
history.go("worx.com");//跳转到最近的worx.com页面
```

也可以使用back和forward来模拟浏览器的后退和前进按钮。