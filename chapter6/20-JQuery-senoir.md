## 第二十节 JQuery 高级


### 20.1 动画

**三种方式显示和隐藏元素：**

1) 默认显示和隐藏方式

1. `show([speed,[easing],[fn])`
   * speed：动画的速度。三个预定义的值("slow","normal", "fast")或表示动画时长的毫秒数值(如：1000)
   * easing：用来指定切换效果，默认是"swing"，可用参数"linear"
      * swing：动画执行时效果是先慢，中间快，最后又慢
      * linear：动画执行时速度是匀速的
   * fn：在动画完成时执行的**函数**，每个元素执行一次。
	
2. `hide([speed,[easing],[fn])`

3. `toggle([speed],[easing],[fn])`

**2) 滑动显示和隐藏方式**（由上到下收起放下）

1. `slideDown([speed],[easing],[fn])`

2. `slideUp([speed,[easing],[fn])`

3. `slideToggle([speed],[easing],[fn])`

**3) 淡入淡出显示和隐藏方式**

1. `fadeIn([speed],[easing],[fn])`

2. `fadeOut([speed],[easing],[fn])`

3. `fadeToggle([speed,[easing],[fn])`


### 20.2 遍历

**1) js 的遍历方式** (jq 也可以使用此方式遍历)

* for(初始化值; 循环结束条件; 步长)

```html
<script type="text/javascript">
    $(function () {
        //1.获取所有的 ul 下的 li
        var citys = $("#city li");
        //2.遍历li
        for (var i = 0; i < citys.length; i++) {
            if("上海" == citys[i].innerHTML){
                //break; 结束循环
                //continue; //结束本次循环，继续下次循环
            }
            //获取内容
            alert(i+":"+citys[i].innerHTML);
        }   
    });   
</script>    
```


**2) jq 的遍历方式**（三种）

1 `jq对象.each(callback)`

 * 语法：`jquery对象.each(function(index,element){});`
    * index：就是元素在集合中的索引
    * element：就是集合中的每一个元素对象
    * this：集合中的每一个元素对象
	
 * 回调函数返回值：
    * true：如果当前 function 返回为 false，则结束循环(break)。	
    * false：如果当前 function 返回为 true，则结束本次循环，继续下次循环(continue)。


```html
<script type="text/javascript">
    $(function () {
        //1.获取所有的 ul 下的 li
        var citys = $("#city li");
        //2. jq对象.each(callback)
        //2.1 获取li对象第一种方式 this，不能获取到索引，空参！
        citys.each(function () {
            alert(this.innerHTML);
            alert($(this).html());
        });  
        //2.2 获取li对象第二种方式，在回调函数中定义参数：index（索引）、element（元素对象）
        //参数名可以起任意值
        citys.each(function (index, element) {
            alert(index+":"+element.innerHTML);
            alert(index+":"+$(element).html());
        }); 
    });   
</script> 
```

2 `$.each(object, [callback])` 

```html
<script type="text/javascript">
    $(function () {
        //1. 获取所有的 ul 下的 li
        var citys = $("#city li");
        //2. $.each(object, [callback])
  		$.each(citys,function () {
			alert($(this).html());
            //其他使用方法参考 `jq对象.each(callback)` 方式
        });
    });   
</script>  
```

3 `for..of`: jquery 3.0 版本之后提供的方式：`for(元素对象 of 容器对象)`

```html
<script type="text/javascript">
    $(function () {
        //1. 获取所有的 ul 下的 li
        var citys = $("#city li");
        //2. for ... of:jquery 3.0 版本之后提供的方式
        for(li of citys){
    		alert($(li).html());
		}
    });   
</script>  
```

### 20.3 事件绑定

1) jquery 标准的绑定方式：`jq对象.事件方法(回调函数);`
* 如果调用事件方法，不传递回调函数，则会触发浏览器默认行为。
* `表单对象.submit();` //让表单提交

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title></title>
    <script src="../js/jquery-3.3.1.min.js" type="text/javascript" charset="utf-8"></script>
    <script type="text/javascript">
        $(function () {
           //1.获取name对象，绑定click事件
           $("#name").click(function () {
               alert("我被点击了...")
           });

           //给 name 绑定两个事件：鼠标移动到元素之上事件、绑定鼠标移出事件。
            $("#name").mouseover(function () {
               alert("鼠标来了...")
            });
            $("#name").mouseout(function () {
                alert("鼠标走了...")
            });

            //简化操作，链式编程
            $("#name").mouseover(function () {
                alert("鼠标来了...")
            }).mouseout(function () {
                alert("鼠标走了...")
            });
            
            alert("我要获得焦点了...")
            //让文本输入框获得焦点，不传参，使用浏览器默认方法获得焦点
            $("#name").focus();
            //表单对象.submit();//让表单提交
        });
    </script>
</head>
<body>
<input id="name" type="text" value="绑定点击事件">
</body>
</html>
```

2) on 绑定事件/off 解除绑定

* `jq对象.on("事件名称",回调函数)`
* `jq对象.off("事件名称")`
* 如果 off 方法**不传递任何参数**，则将组件上的所有事件**全部解绑**

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title></title>
    <script src="../js/jquery-3.3.1.min.js" type="text/javascript" charset="utf-8"></script>
    <script type="text/javascript">
        $(function () {
           //1.使用on给按钮绑定单击事件  click
           $("#btn").on("click",function () {
               alert("我被点击了。。。")
           }) ;
           //2. 使用off解除btn按钮的单击事件
            $("#btn2").click(function () {
                //解除btn按钮的单击事件
                //$("#btn").off("click");
                $("#btn").off();//将组件上的所有事件全部解绑
            });
        });
    </script>
</head>
<body>
<input id="btn" type="button" value="使用on绑定点击事件">
<input id="btn2" type="button" value="使用off解绑点击事件">
</body>
</html>
```

3) 事件切换：toggle

* `jq对象.toggle(fn1,fn2...)`，当单击 jq 对象对应的组件后，会执行 fn1。第二次点击会执行 fn2.....
* 注意：1.9版本 .toggle() 方法删除，jQuery Migrate（迁移）插件可以恢复此功能。

```html
<script src="../js/jquery-migrate-1.0.0.js" type="text/javascript" charset="utf-8">
</script>
```

### 20.4 案例

#### 20.4.1 广告显示和隐藏



#### 20.4.2 抽奖演示



### 20.5 插件

插件：增强 JQuery 的功能

实现方式：

1. `$.fn.extend(object)`，增强通过 Jquery 获取的对象的功能 `$("#id")`。
2. `$.extend(object)`，增强 JQeury 对象自身的功能 `$/jQuery。`



