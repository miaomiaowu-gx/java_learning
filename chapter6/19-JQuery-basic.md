## 第十九节 JQuery 基础

### 19.1 概念

**概念**： 一个 JavaScript 框架。简化 JS 开发

**JavaScript 框架**：本质上就是一些 js 文件，只是封装了 js 的原生代码而已

**jQuery**：是一个快速、简洁的 JavaScript 框架，是继 Prototype 之后又一个优秀的 JavaScript 代码库（或 JavaScript 框架）。jQuery 设计的宗旨是 “write Less，Do More”，即倡导写更少的代码，做更多的事情。它封装 JavaScript 常用的功能代码，提供一种简便的 JavaScript 设计模式，优化 HTML 文档操作、事件处理、动画设计和 Ajax 交互。

### 19.2 快速入门

**1) 下载 JQuery**

🍒 目前 jQuery 有三个大版本：

* `1.x`：兼容 ie678，使用最为广泛的，官方只做 BUG 维护，功能不再新增。
   * 一般项目来说，使用1.x版本就可以，最终版本：1.12.4 (2016年5月20日)

* `2.x`：不兼容 ie678，很少有人使用，官方只做 BUG 维护，功能不再新增。
   * 如果不考虑兼容低版本的浏览器可以使用 2.x，最终版本：2.2.4 (2016年5月20日)

* `3.x`：不兼容 ie678，只支持最新的浏览器。除非特殊要求，一般不会使用 3.x 版本的，很多老的 jQuery 插件不支持这个版本。
   * 目前该版本是官方主要更新维护的版本。最新版本：3.2.1（2017年3月20日）

🍒 **jquery-xxx.js 与 jquery-xxx.min.js 区别**：

1. `jquery-xxx.js`：**开发版本**。给程序员看的，有良好的缩进和注释。体积大一些。

2. `jquery-xxx.min.js`：**生产版本**。程序中使用，没有缩进。体积小一些，程序加载更快。

**2) 导入 JQuery 的 js 文件**：导入 min.js 文件

**3) 使用**

```js
var div1 = $("#div1"); //jq 根据id获取对象
alert(div1.html()); //调用 html() 方法，相当于 js 中的 innerHTML 属性
```

### 19.3 JQuery 对象和 JS 对象区别与转换

1. JQuery 对象在操作时，更加方便。

2. JQuery 对象和 js 对象方法是不通用的。

3. 两者相互转换
   * jq -- > js : `jq对象[索引]` 或者 `jq对象.get(索引)`
   * js -- > jq : `$(js对象)`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JQuer对象和js对象的转换</title>
    <script src="js/jquery-3.3.1.min.js"></script>
</head>
<body>

    <div id="div1">div1....</div>
    <div id="div2">div2....</div>
    
<script>
    //1. 通过js方式来获取名称叫div的所有html元素对象
    var divs = document.getElementsByTagName("div");
    alert(divs.length);//可以将其当做数组来使用
    //对divs中所有的div 让其标签体内容变为"aaa"
    for (var i = 0; i < divs.length; i++) {
        divs[i].innerHTML = "aaa";
    }

    //2. 通过jq方式来获取名称叫div的所有html元素对象
    var $divs = $("div");
    alert($divs.length);//也可以当做数组使用
    //对divs中所有的div 让其标签体内容变为"bbb"  使用jq方式
    $divs.html("bbb");

    /*
        3. 两者相互转换
            * jq -- > js : jq对象[索引] 或者 jq对象.get(索引)
            * js -- > jq : $(js对象)
     */

    for (var i = 0; i < divs.length; i++) {
        // js -- > jq
        $(divs[i]).html("ccc");
    }

    // jq -- > js
    $divs[0].innerHTML = "ddd";
    $divs.get(1).innerHTML = "eee";
    
</script>

</body>
</html>
```


### 19.4 选择器


#### 19.4.1 基本操作学习

**1) 事件绑定**

```js
//1.获取b1按钮
$("#b1").click(function(){
	alert("abc");
});
```

**2) 入口函数**：dom 文档加载完成之后执行该函数中的代码，与 window.onload 功能相似。

```js
$(function () {
});
// 使用示例
$(function () {
    $("#b1").click(function(){
    	alert("abc");
    });  
});
```

**window.onload  和 $(function) 区别**

* `window.onload` 只能定义一次，如果定义多次，后边的会将前边的覆盖掉。
* `$(function)` 可以定义多次的。

**3) 样式控制**：css 方法

```js
$("#div1").css("background-color","red");
$("#div2").css("backgroundColor","pink");
```

#### 19.4.2 分类

**1) 基本选择器**

1. 标签选择器（元素选择器）
   * 语法： `$("html标签名")` 获得所有匹配标签名称的元素，如 `$("div")` 

2. id 选择器 
   * 语法： `$("#id的属性值")` 获得与指定 id 属性值匹配的元素，如 `$("#one")`

3. 类选择器
   * 语法： `$(".class的属性值")` 获得与指定的class属性值匹配的元素，如 `$(".mini")`

4. 并集选择器：
   * 语法： `$("选择器1,选择器2....")` 获取多个选择器选中的所有元素（使用逗号隔开），如 `$("span,#two")`

**2) 层级选择器**

1. **后代**选择器
   * 语法： `$("A B")` 选择 A 元素内部的所有 B 元素，包括子元素、子元素的子元素...

2. **子**选择器
   * 语法： `$("A > B")` 选择 A 元素内部的所有 B 子元素，只有直接的子元素，子元素的子元素不考虑。

**3) 属性选择器**

1. 属性名称选择器 
   * 语法： `$("A[属性名]")` 包含指定属性的选择器。

2. 属性选择器
   * 语法： `$("A[属性名='值']")` 包含指定属性等于指定值的选择器。

3. 复合属性选择器
   * 语法： `$("A[属性名='值'][]...")` 包含多个属性条件的选择器，两个条件同时选中才成立。

```html
 <script type="text/javascript">
     //函数入口，加载后执行
	$(function () { 
		// <input type="button" value="【含有属性】title的div元素背景色为红色" id="b1"/>
		$("#b1").click(function () {
			$("div[title]").css("backgroundColor","pink");
		});
		// <input type="button" value="属性title值【等于】test的div元素背景色为红色" id="b2"/>
		$("#b2").click(function () {
			$("div[title='test']").css("backgroundColor","pink");
		});
		// <input type="button" value="属性title值【不等于】test的div元素(没有属性title的也将被选中)背景色为红色" id="b3"/>
		$("#b3").click(function () {
			$("div[title!='test']").css("backgroundColor","pink");
		});
		// <input type="button" value="属性title值【以te开始】的div元素背景色为红色"  id="b4"/>
		$("#b4").click(function () {
			$("div[title^='te']").css("backgroundColor","pink");
		});
		// <input type="button" value=" 属性title值【以est结束】的div元素背景色为红色" id="b5"/>
		$("#b5").click(function () {
			$("div[title$='est']").css("backgroundColor","pink");
		});
		// <input type="button" value="属性title值【含有es】的div元素背景色为红色" id="b6"/>
		$("#b6").click(function () {
			$("div[title*='es']").css("backgroundColor","pink");
		});
		// <input type="button" value="选取有属性id的div元素，然后在结果中选取属性title值含有“es”的 div 元素背景色为红色" id="b7"/>
		$("#b7").click(function () {
			$("div[id][title*='es']").css("backgroundColor","pink");
		});
	});
</script>
```

**4) 过滤选择器**

1. 首元素选择器 
   * 语法：`:first` 获得选择的元素中的第一个元素

2. 尾元素选择器 
   * 语法：`:last` 获得选择的元素中的最后一个元素

3. 非元素选择器
   * 语法：`:not(selector)` 不包括指定内容的元素

4. 偶数选择器
   * 语法：`:even` 偶数，从 0 开始计数

5. 奇数选择器
   * 语法：`:odd` 奇数，从 0 开始计数

6. 等于索引选择器
   * 语法：`:eq(index)` 指定索引元素

7. 大于索引选择器 
   * 语法：`:gt(index)` 大于指定索引元素

8. 小于索引选择器 
   * 语法：`:lt(index)` 小于指定索引元素

9. 标题选择器
   * 语法：`:header` 获得标题（h1~h6）元素，固定写法

```html
<script type="text/javascript">
	$(function () {
		// <input type="button" value=" 改变第一个 div 元素的背景色为 红色"  id="b1"/>
		$("#b1").click(function () {
			$("div:first").css("backgroundColor","pink");
		});
		// <input type="button" value=" 改变最后一个 div 元素的背景色为 红色"  id="b2"/>
		$("#b2").click(function () {
			$("div:last").css("backgroundColor","pink");
		});
		// <input type="button" value=" 改变class不为 one 的所有 div 元素的背景色为 红色"  id="b3"/>
		$("#b3").click(function () {
			$("div:not(.one)").css("backgroundColor","pink");
		});
		// <input type="button" value=" 改变索引值为偶数的 div 元素的背景色为 红色"  id="b4"/>
		$("#b4").click(function () {
			$("div:even").css("backgroundColor","pink");
		});
		// <input type="button" value=" 改变索引值为奇数的 div 元素的背景色为 红色"  id="b5"/>
		$("#b5").click(function () {
			$("div:odd").css("backgroundColor","pink");
		});
		// <input type="button" value=" 改变索引值为大于 3 的 div 元素的背景色为 红色"  id="b6"/>
		$("#b6").click(function () {
			$("div:gt(3)").css("backgroundColor","pink");
		});
		// <input type="button" value=" 改变索引值为等于 3 的 div 元素的背景色为 红色"  id="b7"/>
		$("#b7").click(function () {
			$("div:eq(3)").css("backgroundColor","pink");
		});
		// <input type="button" value=" 改变索引值为小于 3 的 div 元素的背景色为 红色"  id="b8"/>
		$("#b8").click(function () {
			$("div:lt(3)").css("backgroundColor","pink");
		});
		// <input type="button" value=" 改变所有的标题元素的背景色为 红色"  id="b9"/>
		$("#b9").click(function () {
			$(":header").css("backgroundColor","pink");
		});
	});
</script>
```

**5) 表单过滤选择器**

1. 可用元素选择器 
   * 语法：`:enabled` 获得可用元素

2. 不可用元素选择器 
   * 语法：`:disabled` 获得不可用元素

3. 选中选择器 
   * 语法：`:checked` 获得单选/复选框选中的元素

4. 选中选择器 
   * 语法：`:selected` 获得下拉框选中的元素

```html
<!-- 文本输入框，disabled 属性的框不可输入-->
<input type="text" value="不可用值1" disabled="disabled"> 
<input type="text" value="可用值1" >
<input type="text" value="不可用值2" disabled="disabled">
<input type="text" value="可用值2" >

<br><br>

<!--复选框-->
<input type="checkbox" name="items" value="美容" >美容
<input type="checkbox" name="items" value="IT" >IT
<input type="checkbox" name="items" value="金融" >金融
<input type="checkbox" name="items" value="管理" >管理
 
<br><br>
 
<input type="radio" name="sex" value="男" >男
<input type="radio" name="sex" value="女" >女
  
<br><br>

<!--可多选的下拉框 multiple-->
<select name="job" id="job" multiple="multiple" size=4>
    <option>程序员</option>
    <option>中级程序员</option>
    <option>高级程序员</option>
    <option>系统分析师</option>
</select>

<script type="text/javascript">
	$(function () {
		// <input type="button" value=" 利用 jQuery 对象的 val() 方法改变表单内可用 <input> 元素的值"  id="b1"/>
		$("#b1").click(function () {
			$("input[type='text']:enabled").val("aaa");
		});
		// <input type="button" value=" 利用 jQuery 对象的 val() 方法改变表单内不可用 <input> 元素的值"  id="b2"/>
		$("#b2").click(function () {
			$("input[type='text']:disabled").val("aaa");
		});
		// <input type="button" value=" 利用 jQuery 对象的 length 属性获取复选框选中的个数"  id="b3"/>
		$("#b3").click(function () {
			alert($("input[type='checkbox']:checked").length);
		});
		// <input type="button" value=" 利用 jQuery 对象的 length 属性获取下拉框选中的个数"  id="b4"/>
		$("#b4").click(function () {
			alert($("#job > option:selected").length); //统计下拉框 option 的个数！
            //alert($("#job:selected").length); 永远值为1，因为下拉框只有一个
		});
	});
</script>
```

### 19.5 DOM 操作

**1) 内容操作**

1. `html()`：获取/设置元素的标签体内容  `<a><font>内容</font></a>`  --> `<font>内容</font>`。参数为空则为获取参数值，参数不为空则为设置新值。
2. `text()`：获取/设置元素的标签体纯文本内容  ` <a><font>内容</font></a>` --> 内容。参数为空则为获取参数值，参数不为空则为设置新值。
3. `val()`：获取/设置元素的 value 属性值，参数为空则为获取参数值，参数不为空则为设置新值。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<script  src="../js/jquery-3.3.1.min.js"></script>
		<script>
			$(function () {
                // 获取 myinput 的 value 值
				var value = $("#myinput").val();
				// 设置 value 值
                $("#myinput").val("李四");
                // 获取 mydiv 的标签体内容
				var html = $("#mydiv").html();
				// 设置标签体内容 
                $("#mydiv").html("<p>aaaa</p>");
                // 获取 mydiv 文本内容
                var text = $("#mydiv").text();
                // 🍓设置文本，设置后内容为 <div id="mydiv">bbb</div>，p 标签不存在！
                $("#mydiv").text("bbb");
            });
		</script>
	</head>
	<body>
		<input id="myinput" type="text" name="username" value="张三" /><br />
		<div id="mydiv"><p><a href="#">标题标签</a></p></div>
	</body>
</html>
```


**2) 属性操作**

🍒 通用属性操作

1. `attr()`：获取/设置元素的属性
2. `removeAttr()`：删除属性
3. `prop()`：获取/设置元素的属性
4. `removeProp()`：删除属性
	
* attr 和 prop 区别？
   * 如果操作的是元素的固有属性，则建议使用 prop
   * 如果操作的是元素自定义的属性，则建议使用 attr

🍒 对 class 属性操作

1. `addClass()`：添加 class 属性值
2. `removeClass()`：删除 class 属性值
3. `toggleClass()`：切换 class 属性
   * toggleClass("one")：判断如果元素对象上存在 class="one"，则将属性值 one 删除掉。如果元素对象上不存在 class="one"，则添加
4. `css()`

**3) CRUD 操作**

1. `append()`：父元素将子元素追加到末尾
   * `对象1.append(对象2)`：将对象 2 添加到对象 1 元素内部，并且在末尾。

2. `prepend()`：父元素将子元素追加到开头
   * `对象1.prepend(对象2)`：将对象 2 添加到对象 1 元素内部，并且在开头。

3. `appendTo()`
   * `对象1.appendTo(对象2)`:将对象 1 添加到对象 2 内部，并且在末尾。

4. `prependTo()`
   * `对象1.prependTo(对象2)`:将对象 1 添加到对象 2 内部，并且在开头。

5. `after()`：添加元素到元素后边
   * `对象1.after(对象2)`： 将对象 2 添加到对象 1 后边。对象 1 和对象 2 是兄弟关系。

6. `before()`：添加元素到元素前边
   * `对象1.before(对象2)`： 将对象 2 添加到对象 1 前边。对象 1 和对象 2 是兄弟关系。

7. `insertAfter()`
   * `对象1.insertAfter(对象2)`：将对象 2 添加到对象 1 后边。对象 1 和对象 2 是兄弟关系。

8. `insertBefore()`
   * `对象1.insertBefore(对象2)`： 将对象 2 添加到对象 1 前边。对象 1 和对象 2 是兄弟关系。

9. `remove()`：移除元素
   * `对象.remove()`：将对象删除掉。

10. `empty()`：清空元素的所有后代元素。
    * `对象.empty()`：将对象的后代元素全部清空，但是保留当前对象以及其属性节点。 



### 19.6 案例

