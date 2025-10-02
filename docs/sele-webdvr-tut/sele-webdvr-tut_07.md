# 高级控件

## 高级控件

### 取多个对象

findElements()方法可以返回一个符合条件的元素 List 组，例如：

```
List<WebElement> elements = driver.findElements(By.tagName("input")); 
```

### 层级定位

不方便定位某元素时，可以先定位其父元素，再取父元素的子元素：

```
WebElement element = driver.findElements(By.className("login"));
List<WebElement> elements = element.findElements(By.tagName("label")); 
```

### iframe

网页：

```
<html>
  <head>
    <title>FrameTest</title>
  </head>
  <body style="background-color: #990000;">
    <div id="id1">this is a div!</div>
    <iframe id="frame" frameborder="0" scrolling="no" style="left:0;position:absolute;" src="frame.html"></iframe>
  </body>
</html> 
```

frame.html：

```
<html>
  <head>
    <title>this is a frame!</title>
  </head>
  <body style="background-color: #009900;">
    <div id = "div1">this is a div，too!</div>
    <label>input:</label>
    <input id = "input1"></input>
  </body>
</html> 
```

代码：

```
//在 default content 定位 id="id1"的 div
dr.findElement(By.id("id1"));

//此时，没有进入到 id="frame"的 frame 中时，以下两句会报错
dr.findElement(By.id("div1"));//报错
dr.findElement(By.id("input1"));//报错

//进入 id="frame"的 frame 中，定位 id="div1"的 div 和 id="input1"的输入框。
dr.switchTo().frame("frame");
dr.findElement(By.id("div1"));
dr.findElement(By.id("input1"));

//此时，没有跳出 frame，如果定位 default content 中的元素也会报错。
dr.findElement(By.id("id1"));//报错

//跳出 frame,进入 default content;重新定位 id="id1"的 div
dr.switchTo().defaultContent();
dr.findElement(By.id("id1")); 
```

### 弹出窗口

```
//得到当前窗口的句柄
String currentWindow = dr.getWindowHandle();

//得到所有窗口的句柄
Set<String> handles = dr.getWindowHandles();

for(String handle : handles)
{
  if(currentWindow.equals(handle)) continue;
  WebDriver window = dr.switchTo().window(handle);
  //...
} 
```

### alert、confirm、prompt

*   `getText()` 得到它的文本值
*   `accept()` 相当于点击它的"确认"
*   `dismiss()` 相当于点击"取消"或者叉掉对话框
*   `sendKeys()` 输入值

```
Alert alert = dr.switchTo().alert();
String text = alert.getText();
System.out.println(text);
alert.dismiss(); 
```

```
Alert confirm = dr.switchTo().alert();
String text1 = confirm.getText();
confirm.accept(); 
```

```
Alert prompt = dr.switchTo().alert();
String text2 = prompt.getText();
prompt.sendKeys("jarvi");
prompt.accept(); 
```

### 下拉框

页面：

```
<div id="car-menu">
  <h2>品牌选择</h2>
  <select name="cars",id="select">
    <option value="volvo">Volvo</option>
    <option value="saab">Saab</option>
    <option value="fiat" selected="selected">Fiat</option>
    <option value="audi">Audi</option>
    <option value="bmw">BMW</option>
    <option value="Mercedes Benz ">Mercedes Benz </option>
  </select>
</div> 
```

代码：

```
Select selectCar = new Select(dr.findElement(By.name("cars")));

// 通过下拉列表中选项的索引选中第二项，
selectCar.selectByIndex(4);

// 通过可见文字“audi”选中相应项，
selectengin.selectByVisibleText("audi"); 
```

### 拖放元素

```
WebElement ele = dr.findElement(By.id("item1"));
WebElement tar = dr.findElement(By.id("drop"));
(new Action(dr)).dragAndDrop(ele, tar).perform(); 
```

### 表格

下面这个实例按照原顺序输出表格中的内容：

```
WebElement table = driver.findElement(By.id("my-table"));
List<WebElement> rows = table.findElements(By.tagName("tr"));
for(WebElement row : rows)
{
  // 列里面有"<th>"、"<td>"两种标签，所以分开处理。
  List<WebElement> heads = row.findElements(By.tagName("th"));
  for(WebElement head : heads) 
  {
    System.out.print(head.getText());
    System.out,print(" ");
  }
  List<WebElement> cols = row.findElements(By.tagName("td"));
  for(WebElement col : cols) 
  {
    System.out.print(col.getText());
    System.out,print(" ");
  }
  System.out,println();
} 
```