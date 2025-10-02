# 对浏览器的支持

## 对浏览器的支持

### HtmlUnit Driver

优点：HtmlUnit Driver 不会实际打开浏览器，运行速度很快。对于用 FireFox 等浏览器来做测试的自动化测试用例，运行速度通常很慢，HtmlUnit Driver 无疑是可以很好地解决这个问题。

缺点：它对 JavaScript 的支持不够好，当页面上有复杂 JavaScript 时，经常会捕获不到页面元素。

使用：

```
WebDriver driver = new HtmlUnitDriver(); 
```

### FireFox Driver

优点：FireFox Dirver 对页面的自动化测试支持得比较好，很直观地模拟页面的操作，对 JavaScript 的支持也非常完善，基本上页面上做的所有操作 FireFox Driver 都可以模拟。

缺点：启动很慢，运行也比较慢，不过，启动之后 Webdriver 的操作速度虽然不快但还是可以接受的，建议不要频繁启停 FireFox Driver。

使用：

```
WebDriver driver = new FirefoxDriver(); 
```

Firefox profile 的属性值是可以改变的，比如我们平时使用得非常频繁的改变 useragent 的功能，可以这样修改：

```
FirefoxProfile profile = new FirefoxProfile();
profile.setPreference("general.useragent.override", "some UAstring");
WebDriver driver = new FirefoxDriver(profile); 
```

### InternetExplorer Driver

优点：直观地模拟用户的实际操作，对 JavaScript 提供完善的支持。

缺点：是所有浏览器中运行速度最慢的，并且只能在 Windows 下运行，对 CSS 以及 XPATH 的支持也不够好。

使用：

```
WebDriver driver = new InternetExplorerDriver(); 
```