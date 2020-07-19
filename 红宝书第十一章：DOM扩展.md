# 红宝书第十一章：DOM扩展

		##  一：选择符API

​		**Selectors API Level 1**方法核心：querySelector和querySelectorAll。

​		querySelector方法接收一个CSS选择符，返回与该模式匹配的第一个元素，如果没有返回null；querySelectorAll方法也接收一个CSS选择符，但是返回一个NodeList实例。

​		**Selectors API Level 2**方法为Element 类型新增了一个方法：matchesSelector，这个方法也接收一个CSS选择符，如果调用元素与选择符匹配返回true，否则false。

## 二：元素遍历

​	Element Traversal API为DOM元素添加了下面五种属性：

| 方法名称               | 说明                                     |
| ---------------------- | ---------------------------------------- |
| childElementCount      | 返回子元素的个数（不包含文本节点和注释） |
| firstElementChild      | 返回第一个子元素                         |
| lastElementChild       | 返回最后一个子元素                       |
| previousElementSibling | 指向前一个同辈元素                       |
| nextElementSibling     | 指向后一个同辈元素                       |

## 三：HTML5

​	3.1：与类相关的补充

​	1：getElementByClassName方法：接收一个参数，包含一个或者多个类名的字符串，返回带有指定类的所有元素的NodeList，有多个类名的时候，类名的先后顺序不重要。

​	2：classList属性：为了更加简洁是实现对className属性的相关操作，使用classList的下面方法更加简单：

| 方法名称        | 说明                                     |
| --------------- | ---------------------------------------- |
| add(value)      | 将给定的字符串添加到列表中               |
| contains(value) | 表示列表中是否存在给定值，有返回true     |
| remove(value)   | 从列表删除给定字符串                     |
| toggle(value)   | 如果列表有给定字符串，删除他，否则添加他 |

3.2：焦点管理

​	使用document.activeElement属性，这个属性会始终引用DOM中当前获得焦点的元素。元素获取焦点的方法一般时页面加载，用户输入，和在代码使用**focus**方法。判断元素是否获取焦点使用hasFocus方法。

````javascript
 var button=document.getElementById("my");
	button.focus();
alert(document.activeElement === button);//true
alert(document.hasFocus());//true
````

3.3：HTMLDcoument的变化

​	1：readyState属性

​		这个属性有两个可能的值：loading（正在加载文档），complete（已经加载完文档）

​	2：兼容模式

​		有一个叫做compatMode的属性告诉开发人员浏览器的渲染模式是标准还是混杂的，值也是有两个：CSS1Compat表示标准模式，BackCompat表示混杂模式

3.4：自定义属性

​	在自定义属性的时候前面要加上data-方便辨认，在加入自定义属性后可以使用**dataset**属性来访问值。每一个自定义的 data-形式的属性都有一个对应映射的属性，只不过没有data-前缀，获取自定义（data-myname)的值方法**div.dataset.myname**。

3.5：插入标记

1：innerHTML属性

在读模式下回返回调用元素的所有子节点（包含元素，注释，文本节点）对应的HTML标记。在写模式下，会根据指定的值创建DOM树，然后又这个数完全替换调用节点的所有子节点。

2：outerHTML属性

在读模式，返回调用元素及所有子节点的HTML标签，在写模式下根据指定的HTML字符串创建新的DOM字数，然后用这个DOM字树完全替换调用元素。

3：insertAjacentHTML方法：接收两个参数，插入的位置和要插入的HTML文本

| 第一个参数名称 | 说明                                                   |
| -------------- | ------------------------------------------------------ |
| “beforebegin”  | 当前元素之前插入一个紧邻的同辈元素                     |
| “afterbegin”   | 当前元素之下或者第一个子元素之前插入让一个新的子元素   |
| “beforeend”    | 当前元素之下或者最后一个子元素之后插入让一个新的子元素 |
| “afterend”     | 在当前元素之后擦还让一个紧邻的同辈元素                 |

3.7：scrollIntoView方法

通过滚动浏览器窗口或者某个容器元素，调用元素就会出现在视口，如果传入true或者空值，改元素的顶部就会与视口顶部平齐。