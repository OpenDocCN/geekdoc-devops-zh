# 控件操作

## 控件操作

### 输入框

```
WebElement element = driver.findElement(By.id("passwd-id"));

//在输入框中输入内容：
element.sendKeys(“test”);

//将输入框清空：
element.clear();

//获取输入框的文本内容：
element.getText(); 
```

### 单选框

```
WebElement radio = driver.findElement(By.id("BookMode"));

//选择某个单选项：
radio.click();

//清空某个单选项：
radio.clear();

//判断某个单选项是否已经被选择：
radio.isSelected(); 
```

### 多选框

```
WebElement checkbox = driver.findElement(By.id("myCheckbox"));

//与单选框类似
checkbox.click();
checkbox.clear();
checkbox.isSelected();
checkbox.isEnabled(); 
```

### 按钮

```
WebElement saveButton = driver.findElement(By.id("save"));

//点击按钮：
saveButton.click();

//判断按钮是否 enable:
saveButton.isEnabled (); 
```

### 左右选择框

也就是左边是可供选择项，选择后移动到右边的框中，反之亦然。例如：

```
Select lang = new Select(driver.findElement(By.id("languages")));
lang.selectByVisibleText(“English”);
WebElement addLanguage = driver.findElement(By.id("addButton"));
addLanguage.click(); 
```

### 表单

```
WebElement approve = driver.findElement(By.id("approve"));

approve.click();

//或（只适合于表单的提交）
approve.submit(); 
```

### 文件上传

```
WebElement adFileUpload = driver.findElement(By.id("WAP-upload"));
String filePath = "C:\test\\uploadfile\\media_ads\\test.jpg";
adFileUpload.sendKeys(filePath); 
```