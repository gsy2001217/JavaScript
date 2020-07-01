# 红宝书第三章：ECMAScript基本概念：

​			所有ecmascript区分大小写，开头第一个字符必须是字母，_，或者$；

## 	变量使用：		

​			用var定义的变量将成为定义改变量的作用域的局部变量，也就是只能在改作用域使用，退出后将被销毁，例如：

```javascript
function test(){
	var message="hi";
}
test();
alert(message);//错误，因为message是在test方法定义的，退出即被销毁;
```

如果不在作用域使用var定义，而是直接赋值的话将会产生一个全局变量，可以，但是不推荐；例如：

```javascript
function test(){
	 message="hi";
}
test();
alert(message);//"hi"，因为省略的一个var，从而是message成为全局变量
```

## 	数据类型：

​			ECMAScript有五种数据类型，分别是：Undefined，Null，Boolean，Number，String。还有一种复杂的数据类型Object，本质是由一组无序的名值组成的。ECMAScript所有的数据都具有**动态性**。

### 			Undefined类型：

​		只有一个值，就是undefined，在使用var声明变量但是没有赋值时，变量的值就是undefined（声明但是没有赋值）另外一个报错容易混淆的是“is not defined”这个表示这个变量没有在作用域声明。

### 			Null类型：

​		只有一个值，就是null，表示一个空对象指针

### 			Boolean类型：

​		两个值：true，false。**true不一定等于1，false也不一定等于0**

​		使用Boolean（）函数可以 将一个值转换成对应的Boolean值，转换结果如下：

| 数据类型  | 转换为true的值                 | 转换为false的值 |
| --------- | ------------------------------ | --------------- |
| Boolean   | true                           | false           |
| String    | 任何非空字符串                 | “ ”（空字符串） |
| Number    | 任何非零的数字值（包括无穷大） | 数字0和NaN      |
| Object    | 任何对象                       | null            |
| Undefined | N/A（不适用）                  | undefined       |

### 			Number类型：

​		八进制表示方法：**第一位是数字0，**然后是八进制数字序列（0-7），如果超出范围，第一个的数字0将被忽略，当成十进制解释

​		十六进制表示方法：**前两位必须是0x，**然后是十六进制数字序列（0-9以及a-f（大小写均可））

​		浮点数值，过大或者过小的数字可以使用e表示法，例如：

```
var floatnumber=3.12e7；//等于31200000；
var floatnumber=3.2e-7；//等于0.00000032；
```

由于浮点数存在精度的问题，所以**不要测试某个特点的浮点数值**，这样是无法成功的，下面是错误案例：

```
if(a+b==0.3){
alert("a+b is 0.3");/、不要这样测试，浮点数最高精度是17个小数，所以由于精度误差会导致测试失败
}
```

​		特殊数值：NaN。在ECMAScript里面，任何数值除以0都会得到NaN，因此不会影响代码的运行，他有两点特点：1，任何涉及NaN的操作返回的都是NaN，2：NaN不与任何值相等，包括自己本身。所以在使用siNaN（）函数判断的时候任何不能转换成数值的值都会是这个函数返回true。

​		数值转换的三种方式：**Number（），parseInt（），parseFloat（）；**

​				Number（）转换方式：

| 数据类型  | 结果                                                         |
| --------- | ------------------------------------------------------------ |
| Boolean   | 1/0                                                          |
| 数字值    | 不动                                                         |
| null      | 0                                                            |
| undefined | NaN                                                          |
| 字符串    | 0（字符串为空），包含有效的数字格式则转换成对应的结果，二者都无则是NaN |
| 对象      | 调用valueOf()方法，然后更加前面的规则转换                    |

​				parseInt（）转换方式：第一个参数为所要转换的内容，第二个是所要转换的进制，例如:			

```javascript
var num1=parseInt("AF",16);//175
var num2=parseInt("AF");//NaN
var num3=parseInt("10",2);//2(安装二进制转换)
```

​				parseFloat（）转换方式：从第一个字符开始，知道字符串末尾或者遇到第一个无效的浮点数字字符，即第二个小数点无效。另外，**parseFloat只转换十进制，没有第二个参数作为转换进制的选择**

### 		String类型：

​					**ECMAScript中的字符串是不可变的。**使用toString()方法转换字符串，括号默认无参数，当然也可以通过传递技术来控制toString（）方法输出任意有效进制格式的字符串值，例如：

```javascript
var num=10；
alert(num.toString());//"10"
alert(num.toString(10));//"10"
alert(num.toString(2));//"1010",二进制输出
```

### 		Object类型：

​				每个Object事例都具有以下的属性和方法：

​					1：constructor：保存用于创建当前对象的函数。

​					2：isPrototypeOf（object）：用于检查传入的对象是否是传入对象的原型

​					3：propertyIsEnumerable（propertyNmae）：用于检查给定属性是否可用用for-in语句来枚举。

​					4：hasOwnproperty(propertyName)：用于检查给定的属性在当前的对象是否存在。propertyName作为参数必须是字符串。

​					5：toLocaleString（）：返回对象的字符串表示，该字符串与执行环境的地区对应。

​					6：toString（）：返回对象的字符串表示。

​					7：valueOf（）返回对象的转发 ，数值，布尔值表示。

## 	操作符：

### 						typeof操作符：

​			typeof是一个操作符而不是函数，所以使用的时候直接在后面加上所要检测的数据，例如 

```javascript
var message="hello";
alert(typeof message);
alert(typeof 92);//"number ";
```



### 				一元递增/递减操作符：

​					前置递增或递减，变量的值都是在语句被求值之前改变的。（计算机科学领域中被称为**副效应**）例如：

```javascript
var age=29;
var anotherAge=--age+2;
alert(age);//28
alert(anotherAge);//30
```

由于前置递增和递减操作与执行语句的优先级相等，所以整个语句从左到右被求值，如下：

```javascript
var age=29;
var age2=2;
var age3=--age+age2;//30
var age4=age+age2;//30
```

​					后置递增或者递减实在语句执行之后才执行的，这与前置所不同，二者区别如下：

```javascript
var num=2;
var num1=2;
var num2=20;
var num3=--num1+num2;//21
var num4=num-- + num2;//22
```

### 		布尔操作符：

#### 1，逻辑非（！）转换规则：

| 操作对象    | 返回值 |
| ----------- | ------ |
| 对象        | false  |
| 空字符串    | true   |
| 非空字符串  | false  |
| 数值0       | true   |
| 任意非0数值 | false  |
| null        | true   |
| NaN         | true   |
| undefined   | true   |

#### 2，逻辑与（&&）转换规则：

真值表：

| 第一个操作数 | 第二份操作数 | 结果  |
| ------------ | ------------ | ----- |
| true         | true         | true  |
| true         | false        | false |
| false        | true         | false |
| false        | false        | false |

当有一个操作数不是布尔值的时候遵循的规则：

| 种类                    | 结果                             |
| ----------------------- | -------------------------------- |
| 第一个操作数是对象      | 返回第二个操作数                 |
| 第二个操作数是对象      | 只有在第一个为true时才返回改对象 |
| 两个操作数都是对象      | 返回第二个操作数                 |
| 有一个操作数是null      | 返回null                         |
| 有一个操作数是NaN       | 返回NaN                          |
| 有一个操作数是undefined | 返回undefined                    |

#### 3，逻辑或（||）转换规则：

| 操作数                      | 结果             |
| --------------------------- | ---------------- |
| 第一个操作数是对象          | 返回第一个操作数 |
| 第一个操作数求值结果是false | 返回第二个操作数 |
| 两个操作数都是对象          | 返回第一个       |
| 两个操作数都是null          | 返回null         |
| 两个操作数都是NaN           | 返回NaN          |
| 两个操作数都是undefined     | 返回undefined    |

### 乘法操作符规则：

| 操作数              | 结果                                 |
| ------------------- | ------------------------------------ |
| 都是数值            | 返回数值或者（+-）Infinity（无穷大） |
| 有一个是NaN         | NaN                                  |
| Infinity与数字0想乘 | NaN                                  |
| Infinity与非0相乘   | 结果是Infinity，符合取决于数字       |
| Infinity与Infinity  | Infinity                             |
| 有一个不是数值      | 则在后台调用Number（），在判断       |

### 除法操作符规则：

| 操作数                    | 结果                                 |
| ------------------------- | ------------------------------------ |
| 都是数值                  | 返回数值或者（+-）Infinity（无穷大） |
| 有一个是NaN               | NaN                                  |
| Infinity与数字0相除或被除 | NaN                                  |
| Infinity与Infinity        | Infinity                             |
| 0/0                       | NaN                                  |
| 任意非0数值/0             | Infinity（符号取决于非0数值）        |
| 有一个不是数值            | 则在后台调用Number（），在判断       |

### 	加性操作符：

#### 		一：加法：

​		特殊规则

| 操作符                       | 结果                       |
| ---------------------------- | -------------------------- |
| 两个都是字符串               | 将两个字符串直接拼接       |
| 有一个不是字符串             | 另一个转换成字符串拼接     |
| 有一个是对象，数值或者布尔值 | 利用toString方法转换在拼接 |

#### 		二：减法：

​			特殊规则

| 操作符                                        | 结果                                                         |
| --------------------------------------------- | ------------------------------------------------------------ |
| 有一个操作符是字符串，布尔值，null，undefined | 后台调用Number方法，求值在运算，如果转换为NaN，结果就是NaN   |
| 有一个操作符是对象                            | 调用valueOf方法获取数值，如果没有该方法，使用toString方法将字符串转换成数值 |

### 	关系操作符：

​			种类：(<),(>),(<=),(>=)

### 	相等操作符：

​			1：相等（==），不相等（！=）。

​			2：全等（===），不全等（！==）。

​			区别：全等是指两个操作数在**没有转换类型**便相等（即类型，值都相同），相等是指两个操作数在转换或未转换类型便一样。

### 	条件操作符：

```javascript
	var num=a>b?a:b;//a是否大于b，如果是，则num=啊，否则num=b；
```

### 	赋值操作符：

​				种类：(=),(*=),(/=),(+=),(-=),(<<=)（左移/赋值）,(>>=)（右移/赋值）(>>>=)（无符号右移/赋值）

### 	逗号操作符：

​			可用于声明多个变量；例如var a=1**，**b=2；

## 语句：

​		**for-in语句**是一种精准的**迭代**语句。因为ECMAScript对象的属性没有顺序，所以使用for-in循环输出的值的顺序是**无法预测**的，但是所有值只被返回一次。当迭代的对象变量值为null或者undefined，for-in将会停止循环。

​		**label语句**可以在代码中添加标签，以便将来使用，通常与break和continue一起使用，使用方法：

```javascript
  label:statement;
  例如：
  var count=10;
  var num=0;
  var num1=0;
  start:for(var i=0;i<count;i++){
     for(var j=0;j<5;j++){
         if(i == 5 && j ==2){
             break start;
         }
         num++;
     }
  }
start1:for(var i=0;i<count;i++){
     for(var j=0;j<5;j++){
         if(i == 5 && j ==2){
            continue start1;
         }
         num++;
     }
  }
alert(num);//27
alert(num1);//47
```

​	**with语句**是将代码的作用域设置到一个指定的对象中，语法：with（expression）statement；

这样简化了代码，但是在严格模式下不可以使用，并且大量使用with语句会导致性能下降。

```javascript
//复杂版
var qs=location.search.substring();
var hostName=location.hostname;
var url=location.href;

//使用with语句后
with(location){
    var qs=search.substring();
	var hostName=hostname;
	var url=href;
}
```

​	case语局后面的判断条件可以跟任意数据类型，不一定是数值，比如 ：

```javascript
switch("hello"){
	case "hello":
		alert("你好");
		break;
        
    default:break;
}
```

### 函数：

函数会在执行完return后立刻退出，后面的代码永远无法执行，例如：

```javascript
function sun(a,b){
	return a+b;
	alert("hello");//永远不会执行
}
```

ECMAScript 函数由于不存在**函数签名**的特性，所以不能够重载，当存在多个相同的函数名的函数时候，其名字是属于最后一个函数的。