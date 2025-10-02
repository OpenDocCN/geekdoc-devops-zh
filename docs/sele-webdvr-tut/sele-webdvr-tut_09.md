# 等待

## 等待

### 明确等待

假设被测页面实现了这样的一种效果：点击 click 按钮 4 秒钟后，页面上会出现一个蓝色的 div 块。需要写一段自动化脚本去捕获这个出现的 div，然后高亮它。

```
WebDriverWait wait = new WebDriverWait(dr, 10);
wait.until(new ExpectedCondition<WebElement>() 
{
  public WebElement apply(WebDriver d) 
  {
    return d.findElement(By.cssSelector(".blue_box"));
  }
} 
```

代码 WebDriverWait 类的构造方法接受了一个 WebDriver 对象和一个等待最长时间（10 秒）。然后调用 until 方法，其中重写了 ExpectedCondition 接口中的 apply 方法，让其返回一个 WebElement,即加载完成的元素。默认情况下，WebDriverWait 每 500 毫秒调用一次 ExpectedCondition，直到有成功的返回，当然如果超过设定的值还没有成功的返回，将抛出异常。

### 隐性等待

隐性等待是指当要查找元素，而这个元素没有马上出现时，告诉 WebDriver 查询 Dom 一定时间。默认值是 0,但是设置之后，这个时间将在 WebDriver 对象实例整个生命周期都起作用。

上面的代码可改为如下代码：

```
// 设置 10 秒
dr.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS); 
```