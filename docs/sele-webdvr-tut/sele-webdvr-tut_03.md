# 浏览器操作

## 浏览器操作

### 打开浏览器

打开默认路径的 firefox

```
WebDriver driver = new FirefoxDriver(); 
```

打开指定路径的 firefox

```
System.serProperty("webdriver. firefox.bin",
                   "C:\\Program Files\\Mozilla Firefox\\firefox.exe");
WebDriver driver = new FirefoxDriver(); 
```

或者

```
File pathToFirefoxBinary 
  = new File("C:\\Program Files\\Mozilla Firefox\\firefox.exe");
FirefoxBinary firefoxbin = new FirefoxBinary(pathToFirefoxBinary);
WebDriver driver = new FirefoxDriver(firefoxbin,null); 
```

打开 ie（需要驱动）

```
System.setProperty("webdriver.ie.driver", "...\\IEDriverServer.exe")
WebDriver driver = new InternetExplorerDriver(); 
```

打开 chrome（需要驱动）

```
System.setProperty("webdriver.chrome.driver", "...\\chromedriver.exe" );
System.setProperty("webdriver.chrome.bin", 
                   "C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe");
WebDriver driver = new ChromeDriver(); 
```

### 打开 URL

用 get 方法

```
driver.get("http://www.51.com"); 
```

或者用 navigate 方法，然后再调用 to 方法

```
driver.navigate().to("http://www.51.com"); 
```

### 关闭浏览器

用 quit 方法

```
driver.quit(); 
```

或者用 close 方法

```
driver.close(); 
```

### 返回当前页面 url 和 title

得到 title

```
String title = driver.getTitle(); 
```

得到当前页面 url

```
String currentUrl = driver.getCurrentUrl(); 
```

输出 title 和 currenturl

```
System.out.println(title+"\n"+currentUrl); 
```

### 其他方法

*   `getWindowHandle()` 返回当前的浏览器的窗口句柄
*   `getWindowHandles()` 返回当前的浏览器的所有窗口句柄
*   `getPageSource()` 返回当前页面的源码