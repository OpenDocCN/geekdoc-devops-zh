# 截图

## 截图

```
// 这里等待页面加载完成
Thread.sleep(5000);

// 下面代码是得到截图并保存在 D 盘下
File screenShotFile 
  = ((TakesScreenshot) dr).getScreenshotAs(OutputType.FILE);
FileUtils.copyFile(screenShotFile, 
                   new File("E:/软件测试课程/selenium/test.png")); 
```