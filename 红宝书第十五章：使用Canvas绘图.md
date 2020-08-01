# 红宝书第十五章：使用Canvas绘图

​	canvas元素在页面设定一个JavaScript区域，可以通过JavaScript动态的在里面绘画2D上下文图形

## 一：基本用法

​		首先设置width和height属性，设置完成后也可以通过添加css样式。如果要在这个区域绘画就需要使用**getContext**方法并传入上下文的名字，然后在传入“2d”就可以取得2D上下文对象。使用**toDataURL**方法可以导出在canvas上面绘制的图像，这个方法接收一个参数就是MIME类型。

```javascript
<canvas id="drawing" width="200" height="200">A place for drawing</canvas>

var drawing = document.getElementById("drawing");

if(drawing.getContext){
    var context = drawing.getContext("2d");//2d获取上下文对象
}
```

## 二：2D上下文

​	2D上下文的坐标开始于canvas元素左上角，为（0,0），x，y轴分别向右和向下表示正方向。

### 2.1：填充和描边

​		区分这两个操作的属性分别是：**fillStyle**和**strokeStyle**。这两个属性的值可以是字符串，渐变对象和模式对象，默认值都是“#000000”，可以使用css样式为它们知道颜色格式。

### 2.2：绘制矩形

​		矩形是唯一一种可以直接在2d上下文绘制形状，相关的方法包括：fillReact，strokeRect，clearRect。都接收四个参数：矩形的x坐标，y坐标，高度，宽度，单位都是像素。

​	fillRect方法会在绘制的矩形填充指定的颜色，strokeRect方法在绘制的矩形使用指定的颜色描边，clearRect方法用于清除画布指定矩形区域。

```javascript

var drawing = document.getElementById("drawing");

if(drawing.getContext){
    var context = drawing.getContext("2d");//2d获取上下文对象
    
    context.fillStyle="red";
    context.strokeStyle="yellow";
    context.fillRect(10,10,50,50);//从10,10开始绘制一个50*50的矩形，使用红色填充
    context.strokeRect(10,10,50,50)//从10,10开始绘制一个50*50的矩形，使用黄色描边
   contex.clearRect(30,30,10,10);////从30,30开始清除一个10*10的矩形
}
```

### 2.3：绘制路径

​	首先要调用**beginPath**方法表示开始绘制新路径，然后调用下面方法绘制路径

| 方法名称                                            | 说明                                                         |
| --------------------------------------------------- | ------------------------------------------------------------ |
| arc(x,y,radius,startAngle,endAngle,counterlockwise) | x，y为圆心绘制一条弧线，半径为radius，起始和终止的弧度分别是s，e。c表示是否逆时针方向。 |
| arcTo(x1,y1,x2,y2,radius)                           | 从上一点开始绘制一条弧线，到x2，y2为止，并且以给定半径穿过x1，y1 |
| bezierCurveTo(c1x,c1y,c2x,c2y,x,y)                  | 从上一点开始绘制一条曲线，到x，y为止，两点为控制点           |
| lineTo(x,y)                                         | 从上一点开始绘制一条直线到x，y、                             |
| moveTo(x,y)                                         | 将绘图游标移动到x，y不画线                                   |
| quadraticCurveTo(cx,cy,x,y)                         | 从上一点开始绘制一条二次曲线，到x，y，cxcy作为控制点         |
| rect（x，y，width，height）                         | 从x，y绘制一个矩形                                           |

​		绘制完成后可以使用**closePath**方法绘制一条连接到路径起点的线条，调用**stroke**方法对路径描边，还可以使用**clip**方法在路径上面创建一个剪切区域。

````javascript

var drawing = document.getElementById("drawing");

if(drawing.getContext){
    var context = drawing.getContext("2d");//2d获取上下文对象
   
    //开始路径
    context.beginPath();
    
    //绘制外圆
    context.arc(100,100,99,0,2 * Math.PI,false);
    
    //绘制内圆
    context.moveTo(194,100);
      context.arc(100,100,94,0,2 * Math.PI,false);
    //绘制分针
    context.moveTo(100,100);
    context.lineTo(100,15);
    //绘制时针
    context.moveTo(100,100);
    context.lineTo(30,100);
    
    context.stroke();
}
````

### 2.4：绘制文本

​	绘制文本的主要方法：**fillText**和**strokeText**，接收四个参数：绘制的文本字符串，x坐标，y坐标，可选最大的像素宽度。都含有三个属性：

| 属性名称     | 说明                 |
| ------------ | -------------------- |
| font         | 文本样式，大小，字体 |
| textAlign    | 文本对齐方式         |
| textBaseline | 文本的基线           |

### 2.5：变化

| 方法名称                                 | 说明                                      |
| ---------------------------------------- | ----------------------------------------- |
| rotate（angle）                          | 围绕原点旋转图像angle弧度                 |
| scale（scalex，scaley）                  | 缩放图像                                  |
| translate（x，y）                        | 将坐标原点移动到x，y                      |
| transform（m1，m2，n1，n2，dx，dy，）    | 直接修改变化矩阵                          |
| setTransform（m1，m2，n1，n2，dx，dy，） | 将矩阵重置为默认矩阵后在调用transform方法 |

### 2.6：绘制图像

​	使用**drawImage**方法绘制图像，用三种不同的参数组合：一：传入一个HTML的img元素，加上绘制改图像的起点坐标；二：在这个后面加上两个参数表示其高度和宽度；三：这个方法接收九个参数：要绘制的图像，源图像的坐标，高宽，目标图像的坐标，高宽。

### 2.7：阴影

 2d上下文会根据这几个属性绘制阴影

| 属性名称      | 说明                                  |
| ------------- | ------------------------------------- |
| shadowColor   | 用css颜色格式表示阴影的颜色，默认黑色 |
| shadowOffsetX | 形状或路径x轴方法的阴影偏移量，默认0  |
| shadowOffsetY | 形状或路径y轴方法的阴影偏移量，默认0  |
| shadowBlur    | 模糊的像素数，默认0                   |

### 2.8：渐变

​	 渐变有CanvasGradient实例表示，使用**createLinearGradient**创建新的线性渐变，接收四个参数：起点的坐标，终点的坐标；创建之后可以使用**addColorStop**方法指定色标，这个方法接收两个参数：色标的位置和css颜色值（色标的位置是0开始，1结束）。

````javascript
var drawing = document.getElementById("drawing");
var context=drawing.getContext("2d");
var gradient = =context.createLinearGradient(30,30,70,70);
gardient.addColorStop(0,"white");
gradient.addCOlorStop(1,"black");

context.fillStyle="red";
context.fillRect(10,10,50,50);
//绘制渐变矩形
context.fillStyle= gradient;
context.fillRect(30,30,50,50);
````

​		使用createRadialGradient方法可以创建径向渐变，接收六个参数：起点圆的圆心和半径，终点圆的圆心和半径。

### 2.9：模式

​	模式就是重复的图像，可以填充或者描边图像，调用**createPattern**方法创建，并传入两个参数：一个HTML的img元素和一个表示如何重复图像的字符串（主要包括：repeat，repeat-x，repeat-y，no-repe）。这些都是从（0,0）开始的。

### 2.10：使用图像数据

​	使用getImageDate获取图像原始数据，有三个属性：高宽，和data。data包含：红，绿，蓝，和透明度值，分别按照顺序保存在data内部。

### 2.11：合成

​	globalPlpha和globalComposition-Operation属性应用到所有2d上下文绘制操作中。前者表示绘制的透明度（0到1），后者表示后绘制的图像怎么与先绘制的图像结合。下面的表是后者可能的属性值

| 属性值名称       | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| source-over      | 后绘制的图像在先绘制的图像上方                               |
| source-in        | 后绘制的图像与先绘制的图像重叠部分可见，其他部分透明         |
| source-out       | 后绘制的图像与先绘制的图像不重叠部分可见，先绘制的图像完全透明 |
| source-atop      | 后绘制的图像与先绘制的图像重叠部分可见，先绘制图像不受影响   |
| destination-over | 后绘制的图像位于先绘制的图像下方，只有之前透明像素下的部分才可见 |
| destination-in   | 后绘制的图像位于先绘制的图像下方，两者不重叠的部分完全透明   |
| destination-out  | 后绘制的图像擦除与先绘制的图像重叠的部分                     |
| destination-atop | 后绘制的图像位于先绘制的图像下方，在不重叠的地方先绘制的图像变透明 |
| lighter          | 后绘制的图像与先绘制的图像重叠的部分值相加，使该部分更亮     |
| copy             | 后绘制的图像完全代替重叠的先绘制的图像                       |
| xor              | 后绘制的图像与先绘制的图像重叠的部分执行“异或”操作           |

