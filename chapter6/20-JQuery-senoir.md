## 第二十节 JQuery 高级


### 20.1 动画

**三种方式显示和隐藏元素：**

1) 默认显示和隐藏方式

1. `show([speed,[easing],[fn]])`
   * speed：动画的速度。三个预定义的值("slow","normal", "fast")或表示动画时长的毫秒数值(如：1000)
   * easing：用来指定切换效果，默认是"swing"，可用参数"linear"
      * swing：动画执行时效果是 先慢，中间快，最后又慢
      * linear：动画执行时速度是匀速的
   * fn：在动画完成时执行的函数，每个元素执行一次。
	
2. `hide([speed,[easing],[fn]])`

3. `toggle([speed],[easing],[fn])`
			
**2) 滑动显示和隐藏方式**

1. `slideDown([speed],[easing],[fn])`

2. `slideUp([speed,[easing],[fn]])`

3. `slideToggle([speed],[easing],[fn])`
	
**3) 淡入淡出显示和隐藏方式**

1. `fadeIn([speed],[easing],[fn])`

2. `fadeOut([speed],[easing],[fn])`

3. `fadeToggle([speed,[easing],[fn]])`


### 20.2 遍历

**1) js 的遍历方式**

* for(初始化值;循环结束条件;步长)


**2) jq 的遍历方式**

1. `jq对象.each(callback)`

	* 语法：`jquery对象.each(function(index,element){});`
		* index：就是元素在集合中的索引
		* element：就是集合中的每一个元素对象
		* this：集合中的每一个元素对象
		
	* 回调函数返回值：
		* true：如果当前 function 返回为 false，则结束循环(break)。	
		* false：如果当前 function 返回为 true，则结束本次循环，继续下次循环(continue)。

2. `$.each(object, [callback])`

3. `for..of`: jquery 3.0 版本之后提供的方式：`for(元素对象 of 容器对象)`



### 20.3 事件绑定

1) jquery 标准的绑定方式

* jq对象.事件方法(回调函数)；
* 注：如果调用事件方法，不传递回调函数，则会触发浏览器默认行为。
* 表单对象.submit();//让表单提交

2) on 绑定事件/off 解除绑定

* jq对象.on("事件名称",回调函数)
* jq对象.off("事件名称")
* 如果 off 方法不传递任何参数，则将组件上的所有事件全部解绑

3) 事件切换：toggle

* jq对象.toggle(fn1,fn2...)，当单击 jq 对象对应的组件后，会执行 fn1。第二次点击会执行 fn2.....
* 注意：1.9版本 .toggle() 方法删除,jQuery Migrate（迁移）插件可以恢复此功能。

`<script src="../js/jquery-migrate-1.0.0.js" type="text/javascript" charset="utf-8"></script>`


### 20.4 案例

### 20.5 插件

插件：增强 JQuery 的功能

实现方式：

1. `$.fn.extend(object)`，增强通过 Jquery 获取的对象的功能 `$("#id")`。
2. `$.extend(object)`，增强 JQeury 对象自身的功能 `$/jQuery。`



