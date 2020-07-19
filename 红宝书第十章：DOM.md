# 红宝书第十章：DOM

​	**DOM是针对HTML和xml文档的一个API。**

## 一：节点层次

​	DOM可以将HTML和XML文档描绘成一个由多层节点构成的结构。节点类型主要有：Node，Document，Element，Text，Comment，CDATASection，DocumentFragment，Attr类型。

### 	1.1Node类型

​		DOM1定义了一个Node接口，该接口将由DOM所有的节点类型实现，这个接口在JavaScript中作为Node类型实现的，出来IE其余的浏览器都可以访问这个类型。每个节点都有一个nodeType属性标识节点的类型，在Node类型中用1到12来表示节点类型。

| Node_ELEMENT_NODE(1);                |
| ------------------------------------ |
| Node_ATTRIBUTE_NODE(2);              |
| Node_TEXT_NODE(3);                   |
| Node_CDATA_SECTION_NODE(4);          |
| Node_ENTITY_REFERENCE_NODE(5);       |
| Node_ENTITY_NODE(6);                 |
| Node_PROCESSING_INSTRUCTION_NODE(7); |
| Node_COMMENT_NODE(8);                |
| Node_DOCUMENT_NODE(9);               |
| Node_DOCUMENT_TYPE_NODE(10);         |
| Node_DOCUMENT_FRAGMENT_NODE(11);     |
| Node_NOTATION_NODE(12);              |

​	1.1nodeName和nodeValue属性

​		想要了解节点的具体信息，可以通过这两个属性，但是这两个属性取决于节点的类型。

​	1.2节点关系

​		每个节点都有一个childNodes属性，保存一个NodeList对象，这个对象用于保存一组有序的节点，可以通过为止来访问这些节点，并且它还具有length属性，但是他不是Array实例。每个节点都有一个parentNODE属性，这个属性指向文档树中的父节点，包含在childNodes列表中的每个节点相互之间都是同胞节点，使用previousSibling和nextSibling属性是可以访问到同一个列表的其他列表节点。

​	1.3操作节点

​		因为关系节点只是可读，所以可以通过使用appendChild（）方法来向**childNodes列表的末尾添加一个节点**。如果想要在特定的地方插入节点，那么需要使用insertBefore方法，这个方法接收两个参数：要插入的节点和作为参展的节点，**插入后该节点会变成参照节点的前一个节点**。如果插入的节点是null的话，那么两个方法执行相同的操作。

```javascript
//someNode有多个节点

//插入后成为最后一个子节点
returnNode =insertBefore(newNode,null);
alert(newNode== someNode.lastChild);//true

//插入后成为第一个子节点
returnNode =insertBefore(newNode,somdeNode.firstChild);
alert(returnNode==newNode);//true
alert(newNode == someNode.firstChild);//true

//插入到最后一个子节点的前面
returnNode =insertBefore(newNode,somdeNode.lastChild);
alert(newNode ==someNode.childNodes[someNode.lastChild.length-2]);
/**true  someNode.childNodes[someNode.lastChild.length-1]表示的是someNode子列表的最后一个节点
*/
```

​		replaceChild（）方法接收两个参数，第一个是要插入的节点，第二个是要替换的节点，这个要替换的节点将被这个方法从文档树中删除，同时又插入的节点代替他的位置。使用removeChild方法可以直接移除一个节点，被移除的节点将变成方法的返回值。

​		**注意：以上的四个方法在使用的时候必须先取得父节点。**

​	1.4其他方法

​		所有类型节点都具有的两个方法：一：cloneNODE方法，接收一个布尔值来表示是否进行深复制，true的情况下复制节点及其整个子节点树，当时false的时候只复制节点本身

### 		1.2Document类型

​		Document节点具有这些特点：nodeType的值是9，nodeName的值是“#document”，nodeValue的值是null，parentNode的值是null，ownerDocument的值是null，它的子节点可能DocumentType（最多一个）也可能是E了美团，ProcessingInstruction或者Comment。Document对象时window对象的一个属性，因此他也可以当做一个全局对象来访问。

​		2.1文档的子节点

​	可以通过documentElement属性访问子节点，该属性始终执行html界面的<html>元素，另外一个便是通过childNodes列表访问文档元素。

​		2.2文档信息

​	作为一个HTMLDocument的实例，document对象有一些标准的Document对象么有的属性。title属性：可以将元素中的文本显示在浏览器的标题栏上面，URL属性：包含页面完整的路径，domain属性值包含页面的域名，referrer属性则保持着链接到当前页面的那个页面的URL。我们都可以使用document方法来访问到这些数据。这几个属性只有domain属性时可以被设置的，但是不能把这个值设置成URL所不包含的域。

​		2.3查找元素

​	使用getElementById和getElementByTagName来获取特定的元素。还有一个是只有HTMLDocument类型才有的，那就是getElementByName，会返回给定name特性的所有元素。

​		2.4特殊集合

​	还提供了一下属于HTMLCollection对象的集合，为访问文档常用的步伐提供了快捷方式

| 方法名           | 说明                              |
| ---------------- | --------------------------------- |
| document.anchors | 包含文档中所有带name特性的<a>元素 |
| document.applets | 包含文档所有的<applet>元素        |
| document.forms   | 包含文档所有的<form>元素          |
| document.images  | 包含文档中所有的<img>元素         |
| document.links   | 包含文档中所有带href特性的<a>元素 |

​		2.5文档写入

​	主要使用四个方法：write，writeln，open，close。

write()方法是直接将接受的参数写入，但是writeln方法会在字符串的末尾加上一个换行符。open和close则是分别用于打卡和关闭网页的输出流，如果实在页面加载期间使用write或者writeln方法则不需要使用这两个方法。

### 	1.3：Element类型

​		Element节点的特征：nodeType的值为1，nodeName的值是null，nodeValue的值是null，parentNode可能是Document或者Element，子节点可能是Element，Text，Comment，ProcessingInstruction

​	3.1：HTML元素

​	所有的HTML元素都有HTMLElement类型显示，每个HTML元素都存在闲下来的标准特性

| 名称      | 说明                                     |
| --------- | ---------------------------------------- |
| id        | 元素在文档的唯一标识符                   |
| title     | 有关元素的附近说明信息                   |
| lang      | 元素内容的语言代码，很少使用             |
| dir       | 语言的方向                               |
| className | 与元素class特性对应，即为元素指定的css类 |

​	3.2：取得特性

​		每一个元素都有特性，操作这些特性的信息的方法主要是下列三种：getAttribute，setAttribute，removeAttribute。特性的名称是不区分大小写的，自定义属性的话应该加上data-前缀以便验证。第一个接收一个参数，参数是你要获取特性的名称；第二个是设置特性，接收两个参数，第一个是要设置的特性名称，第二个是对应的值；第三个就是删除元素的特性，接收一个参数就是特性名称。

​	3.4：attribute属性

​		Element类型是使用attribute属性的唯一DOM节点类型。attribute属性包含一个NamedNodeMap。是一个“动态”的集合，元素的每个特性都有一个Attr节点表示。NamedNodeMap所拥有的方法有

| 方法名称                | 说明                                         |
| ----------------------- | -------------------------------------------- |
| getNamedItem（name）    | 返回nodeName属性等于name的节点               |
| removeNamedItem（name） | 那个列表删除nodeName属性等于name的节点       |
| setNamedItem（node）    | 想列表添加节点，以节点的nodeName属性作为索引 |
| item（pos）             | 返回位于数字pos位置的节点                    |

​	3.5：创建元素

​		使用document.createElement方法创建新元素，接收一个参数，级要创建元素的标签名。丹由于新元素没有被加入到文档树中，所以浏览器不会显示这些特性，我们需要使用appendChild，insertBefore或者replaceChild方法来实现。

​	3.6：元素的子节点

​		元素可以拥有任意数目的子节点和后代节点，因为元素可以是其他元素的子节点。因为不同的浏览器在对待相同的代码时处理结果不同，所以我们在执行某些操作之前判断nodeType 类型。

```javascript
for(var i=0,len=element.childNodes.length;i<len;i++){
    if(element.childNodes[i].nodeType==1){
        //如果是元素节点就执行某些操作
    }
}
```

### 1.4：Text类型

 		文本节点由Text类型表示节点，包含的可以是纯文本内容，也可以是转义后的HTML字符，但是不可以是HTML代码。Text节点的特征。

| nodeType的值是3               |
| ----------------------------- |
| nodeName的值是“#text”         |
| nodeValue的值是节点包含的文本 |
| parentNode是一个Element       |
| 没有子节点                    |

​		可以使用下面的方法来操作节点中的文本

| 方法名称                           | 说明                                             |
| ---------------------------------- | ------------------------------------------------ |
| appendData（text）                 | 将text添加到节点的末尾                           |
| deleteData（offset，count）        | 从offset指定的位置开始删除count个字符            |
| insertData（offset，text）         | 从offset指定的位置开始插入text                   |
| replaceData（offset，count，text） | 从offset指定位置开始替换长度为count位置的text    |
| splitText（offset）                | 在offset指定的位置把当前文本节点分为两个文本节点 |
| substringData（offset，count)      | 从offset指定位置开始提取长度为count的字符串      |

​		为了规范化文本节点，所有的节点都含有一个叫做normalize的方法，他可以让多个文本子节点拼接起来，与之相反的便是aplitText方法，他是把文本子节点分成两个子节点。

```javascript
var element=document.createElement("div");
element.className="message";

var textNode=document.createTextNode("hello world!");
element.appendChild(textNode);

var textNode1=document.createTextNode("lihaua");
element.appendChild(otextNode1);

document.body.appendChild(element);

element.normalize();
alert(element.firstChild.nodeValue);//"hello world!lihua"

var newNode=element.firstChild.splitText(5);
alert(element.firstChild.nodeValue);//"hello";
```

### 1.5：DocumentFragment类型

​		所有的节点类型只有它在文档中没有对应的标记DOM规定 文档片段（document fragment）是一种轻量级的文档，可以包含和控制节点但是不会占用额外的资源，。虽然不能直接将文档片段添加到文档中，但是鉴于把他作为一个仓库使用，存贮可能会添加到文档的节点，使用案例

```javascript
var fragment=document.createDocumentFragment();
var ul=document.getElemnetById("mylist");
var li=null;

for(var i=0;i<3;i++){
    li=document.createElement("li");
    li.appendChild(document.creatTextNode("Item"+i));
    fragment.appendChild(li);
}

ul.appenChild(fragment);
```



