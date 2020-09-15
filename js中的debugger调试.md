# js中的debugger调试

debugger：停止JS的执行，相当于设置断点。
在JS代码编写的过程中，我们都会通过浏览器的调试模式（F12）来检查代码是否正确，大多数我们都是通过设置断点来进行调试。
打开浏览器按F12：

![](D:\web\Typora\bug\static\pic\debugger01.png)

在12行设置断点（鼠标点击12）：

![](D:\web\Typora\bug\static\pic\debugger02.png)

按F5刷新界面（当前浏览器会执行你设置断点的位置的时候）：

![](D:\web\Typora\bug\static\pic\debugger03.png)

然后按F10一步一步执行下去，这是我们传统的JS调试方法，但是如果遇见JS代码过多并且杂乱的时候（比如上千行的时候），我们自己找位置设置断点的时候就会发现每次都要向下滑一会儿；要么就ctrl+F查找（可能出现相同的变量等等情况）；或者记住当前代码编写的行数位置，再在浏览器调试模式中滑到相应的位置设置断点，总感觉很烦。

这个时候我们就可以使用JS中提供的debugger语句：

![](D:\web\Typora\bug\static\pic\debugger04.png)

按F5刷新：

![](D:\web\Typora\bug\static\pic\debugger05.gif)

可以看见JS在执行的时候会自动命中debugger然后设置断点，在大型项目中，这样会更加方便我们在JS中的调试！！！



通常我们在网站开发的过程中会用到很多JavaScript代码，
来进行调试，调试方法有很多，以前用的是火狐浏览器的调试工具，但是后来版本等等的原因不能用了，
现在就用debugger在谷歌浏览器中进行调试。
如下一段js代码：

```javascript
![debugger06](D:\web\Typora\bug\static\pic\debugger06.png)//查询方法实现
    function search(){ 
        debugger；
        var url = '${base}oa/dbkh!listdata.action';
        var startTime=$('#startTime').val();

        if(startTime==null){
            startTime="";
        }
        var username =$('#username').val(); 

        jQuery("#role_list_table").jqGrid('setGridParam', {
            url :"${base}oa/dbkh!listdata.action?endStr = " + 1 + "&startTime= " + startTime+ "&username= " + username,
            page:curpagenum
        }).trigger("reloadGrid");  
}
```

在谷歌浏览器中运行项目，打开F12，然后输入条件，并且点击“查询”按钮，回到如下图：

![](D:\web\Typora\bug\static\pic\debugger06.png)

然后使用F8，F10，F11进行调试,如下图：

调试过程中会下实处变量的值等等信息，这样在找bug的过程中就会方便的多。

