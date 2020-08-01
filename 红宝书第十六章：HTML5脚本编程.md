# 红宝书第十六章：HTML5脚本编程

## 一：跨文本消息传递

​	简称是**XDM**，核心方法是**postMessage**方法，这个方法是为了向另外一个地方传递数据。这个方法接收两个参数：一个信息，和一个表示信息来自哪个域的字符串。

````javascript
var iframe = document.getElementById("maframe").contentWindow;
iframe.postMessage("a","http://www.ss.com");
````

## 二：原生拖放

​		**2.1：拖放事件**

​	在拖动某个元素的时候会依次触发dragstart，drag，dragend事件。当按下鼠标键并且开始移动鼠标触发第一个事件，这个时候可以通过**ondragstart**事件处理程序运行JavaScript代码；然后紧接着触发drag事件，拖放停止是触发dragend事件。

​	当某个元素被拖动到一个有效的放置目标时会依次触发：dragenter，dragover，dragleave事件。

​	**2.2：自定义放置目标**

​	重写dragenter和dragover事件的默认行为可以是任何元素变成有效的放置目标。方法如下

```javascript
var drop = document.getElementById("my");

EventUtil.addHandler(drop,"dragenter",function(event){
    EventUtil.preventDefault(event);
});
EventUtil.addHandler(drop,"dragover",function(event){
    EventUtil.preventDefault(event);
});
```

​	**2.3：dataTransfer对象**

​	只能在拖动事件的事件处理程序访问这个对象，这个对象有两个方法：getData和setData方法，接收两个参数：数据保存的类型text或者URL，保存的数据（一个字符串）。

​	**2.4：dropEffect和effectAllowed**

​	利用dataTransfer对象不仅仅可以传输数据，还可以确定被拖动的元素以及放置目标可以接收什么操作，需要访问两个属性：dropEffect，effectAllowed。

​	通过dropEffect属性可以知道被拖动的元素可以执行那种操作。

| 名称 | 说明                           |
| ---- | ------------------------------ |
| none | 不能把拖动的元素放在这里       |
| move | 应该把拖动的元素移动到放置目标 |
| copy | 应该把拖动的元素复制到放置目标 |
| link | 表示放置目标会打开拖动的元素   |

但是dropEffect必须搭配effectAllowed属性使用，后者表示允许拖动元素的哪种dropEfect。

| 值名称        | 说明                              |
| ------------- | --------------------------------- |
| uninitialized | 没有给被拖动的元素设置任何行为    |
| none          | 被拖动的元素不允许有任何行为      |
| copy          | 只允许值为“copy”的dropEffect      |
| link          | 只允许值为“link”的dropEffect      |
| move          | 只允许值为“move”的dropEffect      |
| copyLink      | 允许值为copy或者link 的dropEffect |
| copyMove      | 允许值为copy或者move的dropEffect  |
| linkMove      | 允许值为link或者move的dropEffect  |
| all           | 允许任意的dropEffect              |

​	**2.5：可拖动**

​	HTML5位所有的HTML元素规定了一个draggble属性表示是否可用被拖动，只有图像和链接的被设置为了true。

## 三：媒体元素

​	在网页嵌入跨浏览器的音频和视频内容的标签是：audio和video。使用的时候至少加入src属性，还可以设置width和height属性表示播放器的大小，**poster**属性指定图像的URL可用在加载视频内容期间显示另外一幅图像。如果含有**control**属性表示应该还有UI控件。

​	使用audio和video元素的**play**和**pause**方法可以手工的控制每天文件的播放，组合使用元素和事件和这两个方法可以轻易的创建一个自定义的媒体播放器。

````javascript
<div>
    <div class="video">
        <video id="player" src="" poster="" width="300" height="2000">
        	video
        </video>    
    </div>
	<div class="controls">
		<input type="button" value="play" id="video-btn">
		<span id="curtime">0</span>/<span id="duration">0</span>
	</div>
</div>

var player=document.getElementById("player"),
    btn=document.getElementById("video-btn"),
    curtime=document.getElementById("curtime"),
    duration=document.getElementById("duration");
	duration.innerHTML=player.duration;//更新播放时间
	EventUtil.addHandler(btn,"click",function(event){
        if(player.paused){
        player.play();
        btn.value = "Pause";
        }else{
            player.pause();
            btn.value="Play";
        }
    });
//更新播放时间
setInterval(function(){
    curtime.innerHTML = player.currentTime;
},200);
````



