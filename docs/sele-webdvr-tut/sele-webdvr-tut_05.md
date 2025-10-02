# 选择器

## 选择器

### By ID

页面：

```
<input type="text" name="passwd" id="passwd-id" /> 
```

代码：

```
WebElement element = driver.findElement(By.id("passwd-id")); 
```

### By Name

页面同上

代码：

```
WebElement e = dr.findElement(By.name("passport_51_user")); 
```

### By XPATH

```
WebElement element 
  = driver.findElement(By.xpath("//input[@id='passwd-id']")); 
```

### By Class Name

页面：

```
<div class="cheese">
  <span>Cheddar</span>
</div>
<divclass="cheese">
  <span>Gouda</span>
</div> 
```

代码：

```
List<WebElement> cheeses 
  = driver.findElements(By.className("cheese")); 
```

### By Link Text

页面：

```
<a href="http://www.google.com/search?q=cheese">cheese</a> 
```

代码：

```
WebElement cheese = driver.findElement(By.linkText("cheese")); 
```