# 鼠标键盘模拟

## 鼠标键盘模拟

### 单一操作

```
//新建一个 action
Actions action=new Actions(driver);

//操作
WebElement element = dr.findElement(By.id("test"));
WebElement element1 = dr.findElement(By.id("su"));
action.sendKeys(element,"test").perform();
action.moveToElement(element1);
action.click().perform(); 
```

### 组合操作

```
(new Actions(dr)).dragAndDrop(dr.findElement(By.id(item)), target).perform();
Action dragAndDrop = builder.clickAndHold(someElement)
  .moveToElement(otherElement)
  .release(otherElement)
  .build().perform(); 
```

其他鼠标或键盘操作方法可以具体看一下 API 里面的`org.openqa.selenium.interactions.Actions`类。