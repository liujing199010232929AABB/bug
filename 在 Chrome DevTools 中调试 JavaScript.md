# 在 Chrome DevTools 中调试 JavaScript

由浅入深说一说怎么样在 Chrome DevTools 中调试 JavaScript。

## 一、案发现场

为了方便理解，我写了一个小demo。

1. 点击[**打开demo**](https://caomage.com/breakpoint)；
2. 在num1中输入6；
3. 在num2中输入9；
4. 点击 num1+num2，按钮下方的标签显示 69，结果应为 15，这就是我们需要断点调试找出的 BUG 。

![](D:\web\Typora\bug\static\pic\chrome_debugger01.png)

## 二、熟悉一下 Sources 面板

DevTools 可为更改 CSS、分析页面加载性能和监控网络请求等不同的任务提供许多不同的工具。 我们就在 Sources 面板中调试 JavaScript。

通过按 `Command+Option+I` (Mac) 或 `Control+Shift+I`（Windows、Linux），打开 DevTools。 此快捷方式可打开 **Console** 面板。点击 **Sources** 面板。

Sources 面板包含 **3 个部分**：

![](D:\web\Typora\bug\static\pic\chrome_debugger02.png)

1. **文件预览** 窗口。 此处列出页面请求的每个文件。
2. **代码编辑** 窗口。 在 **文件预览** 窗口中选择文件后，此处会显示该文件的具体内容。
3. **JavaScript 调试** 窗口。 包含检查页面 JavaScript 的各种工具。 如果 DevTools 窗口布局较窄，此窗口会显示在 **代码编辑** 窗口下方。

## 三、使用断点暂停代码

调试上面这种问题的常用方法是将多个 `console.log()` 语句插入代码，以便在执行脚本的时候检查相关变量的值。

虽然 `console.log()` 方法可以完成任务，但断点可以更快完成此任务。 断点可在执行代码的过程中暂停代码，并在此时及时检查所有相关变量的值。 与 `console.log()` 方法相比，**断点具有一些优势：**

1. 使用 `console.log()`，需要手动打开源代码，查找相关代码，插入 `console.log()` 语句，然后重新加载此页面，才能在控制台中看到这些消息。 使用断点，无需了解代码结构即可暂停相关代码。
2. 在 `console.log()`语句中，您需要明确指定要检查的每个值。 使用断点，DevTools 会在暂停时及时显示所有变量值。
3. 简言之，与 `console.log()` 方法相比，断点可以更快地查找和修正 BUG 。

接下来我们开始思考一开始抛出的程序的运作方式，我们可以根据经验推测出，我们在点击num1+num2按钮的时候触发的 click 事件肯定和 6+9=69 计算不正确有关系。 因此，我们可能需要在 click 侦听器运行的时候暂停代码。
`Event Listener Breakpoints` 可以完成此任务：

1. 在 **JavaScript 调试** 窗口中，点击 `Event Listener Breakpoints` 前面的展开按钮。 可以看见 *Animation、Canvas、Clipboard* 等一系列事件；

2. 在页面输入框中输入num1和num2的值；

3. 展开 Mouse 事件，每个事件旁都有一个复选框。勾选 click 复选框。 DevTools 现在可以在任何 click 事件侦听器运行时自动暂停。

4. 点击页面中的num1+num2按钮。此时页面如下图：

   ![](D:\web\Typora\bug\static\pic\chrome_debugger03.png)

## 四、检查变量的值

### 1. Scope窗口

在某代码行暂停时，Scope 窗格会显示当前定义的局部和全局变量，以及各变量值。 其中还会显示闭包变量（如果适用）。 双击变量值可进行编辑。 如果不在任何代码行暂停，则 Scope 窗格为空。

![](D:\web\Typora\bug\static\pic\chrome_debugger04.png)

### 2. Watch监听变量变化

Watch 标签可监视变量值随时间变化的情况。 并且，监视不仅限于监视变量。 我们可以将任何有效的 JavaScript 表达式存储在监视表达式中。 我们尝试这样：
\- 点击 Watch 标签。
\- 点击 右边的 **+** 添加表达式。
\- 输入 `typeof n`。 按 Enter 键。（这里代码是打包后的，n表示num1输入框的值）
\- DevTools 会显示 `typeof n: "string"`。 冒号右侧的值就是监视表达式的结果。

![](D:\web\Typora\bug\static\pic\chrome_debugger05.png)

### 3. 控制台

控制台除了查看 `console.log()` 消息以外，还可以使用控制台对任意 JavaScript 语句求值。 对于调试，可以使用控制台测试 BUG 的潜在**解决方法**:

```javascript
在 Console 中，输入 `parseInt(n) + parseInt(u)`。 此语句有效，因为我们会在特定代码行暂停，其中 `n`（num1的值） 和 `u`（num2的值） 在范围内。

按 Enter 键。 DevTools 对语句求值并打印输出 15，即我们预计demo页面会产生的结果。
```

![](D:\web\Typora\bug\static\pic\chrome_debugger06.png)

## 五、尝试修改

上一步我们已找到解决 BUG 的方法。 接下来就是尝试通过编辑代码并重新运行demo来使用修正方法。 我们可以在 **代码编辑** 窗口直接修改代码：

在 **代码编辑** 窗口中，将代码格式化关掉，然后修改代码，将 `n+u` 换成 `parseInt(n)+parseInt(u)` 。

![](D:\web\Typora\bug\static\pic\chrome_debugger07.png)

按 `Command+S` (Mac) 或 `Control+S`（Windows、Linux）以保存更改。

点击 Deactivate breakpoints 取消激活断点。 其将变为蓝色，表示处于活动状态。 在完成此设置后，DevTools 会忽略您已设置的任何断点。

![](D:\web\Typora\bug\static\pic\chrome_debugger08.png)

点击num1+num2按钮，则会看见正确的结果啦！

*Tips: 这样做只能修正在浏览器中运行的代码， 不能为访问您页面的所有用户修正代码。 为此，我需要修改自己服务器上的代码。*

## 六、介绍其他几种断点

| 断点类型   | 使用场景                                     |
| ---------- | -------------------------------------------- |
| 代码行     | 在确切的代码区域中                           |
| 条件代码行 | 在确切的代码区域中，且仅当其他一些条件成立时 |
| DOM        | 在更改或移除特定 DOM 节点或其子级的代码中    |
| XHR        | 当 XHR 网址包含字符串模式时                  |
| 事件侦听器 | 在触发 click 等事件后运行的代码中            |
| 异常       | 在引发已捕获或未捕获异常的代码行中           |
| 函数       | 任何时候调用特定函数时                       |

### 1. 代码行断点

1. 直接点击
   这是使用最多的一种断点方式，在知道需要检查的确切代码区域时，可以使用代码行断点。 DevTools 始终会在执行此代码行之前暂停。

   ![](D:\web\Typora\bug\static\pic\chrome_debugger09.png)

`2.debugger`
在代码中调用 `debugger` 可在该行暂停。 此操作相当于使用代码行断点，只是此断点是在代码中设置，而不是在 DevTools 界面中设置。

```html
console.log('a');
console.log('b');
debugger;
console.log('c');
```

3.条件代码断点
如果知道需要调查的确切代码区域，但只想在其他一些条件成立时进行暂停，则可使用条件代码行断点。若要设置条件代码行断点：

1. 点击 Sources 标签。
2. 打开包含您想要中断的代码行的文件。
3. 转至代码行。
4. 代码行的左侧是行号列。
5. 右键点击行号列。
6. 选择 Add conditional breakpoint。
7. 代码行下方将显示一个对话框。
8. 在对话框中输入条件。
9. 按Enter 键激活断点。 行号列顶部将显示一个橙色图标。

![](D:\web\Typora\bug\static\pic\chrome_debugger10.png)

### 2. DOM更新断点

如果想要暂停更改 DOM 节点或其子级的代码，可以使用 DOM 更改断点。若要设置 DOM 更改断点：

- 点击 Elements 标签。
- 转至要设置断点的元素。
- 右键点击此元素。
- 将鼠标指针悬停在Break on 上，然后选择 Subtree modifications、Attribute modifications 或 Node removal。

![](D:\web\Typora\bug\static\pic\chrome_debugger11.png)

- **Subtree modifications**： 在移除或添加当前所选节点的子级，或更改子级内容时触发这类断点。在子级节点属性发生变化或对当前所选节点进行任何更改时不会触发这类断点。
- **Attributes modifications**：在当前所选节点上添加或移除属性，或属性值发生变化时触发这类断点。
- **Node Removal**：在移除当前选定的节点时会触发。

### 4. XHR/Fetch断点

如果想在 XHR 的请求网址包含指定字符串时中断，可以使用 **XHR 断点**。 DevTools 会在 XHR 调用 `send()` 的代码行暂停。

注：此功能还可用于 **Fetch** 请求。

例如，在您发现您的页面请求的是错误网址，并且您想要快速找到导致错误请求的 **AJAX** 或 **Fetch** 源代码时，这类断点很有用。

若要设置 XHR 断点：

- 点击 Sources 标签。
- 展开 XHR Breakpoints 窗格。
- 点击 Add breakpoint。
- 输入要对其设置断点的字符串。 DevTools 会在 XHR 的请求网址的任意位置显示此字符串时暂停。
- 按 Enter 键以确认。

![](D:\web\Typora\bug\static\pic\chrome_debugger12.png)

这样就可以拦截包含`getUserInfo`字符串的请求，如果添加一个空的，则可以拦截所有请求！

### 5. 事件侦听器断点

如果想要暂停触发事件后运行的事件侦听器代码，可以使用事件侦听器断点。 您可以选择 click 等特定事件或所有鼠标事件等事件类别。

我们一开始使用的例子就是事件侦听器断点，这里就不演示了。

### 6. 异常断点

如果想要在引发**已捕获**或**未捕获**异常的代码行暂停，可以使用异常断点。

- 点击 Sources 标签。
- 点击 Pause on exceptions 引发异常时暂停 {:.devtools-inline}。 启用后，此按钮变为蓝色。
- （可选）如果除未捕获异常以外，还想在引发已捕获异常时暂停，则勾选 Pause On Caught Exceptions 复选框。

![](D:\web\Typora\bug\static\pic\chrome_debugger13.png)

### 7. 函数断点

如果想要在调用特定函数时暂停，可以调用 `debug(functionName)`，其中 `functionName` 是要调试的函数。 您可以将 `debug()` 插入您的代码（如 `console.log()` 语句），也可以从 DevTools 控制台中进行调用。 `debug()` 相当于在第一行函数中设置代码行断点。

```javascript
function sum(a, b) {
  let result = a + b; // DevTools 会在此行暂停
  return result;
}
debug(sum); // 传递函数对象，而不是字符串。
sum();
```

如果想要调试的函数不在范围内，DevTools 会引发 `ReferenceError`。

```javascript
(function () {
  function hey() {
    console.log('hey');
  }
  function yo() {
    console.log('yo');
  }
  debug(yo); // 这行可以成功调用
  yo();
})();
debug(hey); // 这一行不能成功调用 hey() 不在作用域内
```

如果是从 DevTools 控制台中调用 `debug()`，则很难确保目标函数在范围内。所以一般还不如直接使用代码行断点！