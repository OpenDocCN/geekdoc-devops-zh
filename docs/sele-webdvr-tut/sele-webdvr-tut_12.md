# 其它

## 其它

### firefox 代理

```
String PROXY = "localhost:8080";//如果不是本机，localhost 替换成 IP 地址
org.openqa.selenium.Proxy proxy = new org.openqa.selenium.Proxy();
proxy.setHttpProxy(PROXY)
  .setFtpProxy(PROXY)
  .setSslProxy(PROXY);
DesiredCapabilities cap = new DesiredCapabailities();
cap.setPreference(CapabilityType.PROXY, proxy);
WebDriver driver = new FirefoxDriver(cap); 
```

### 启用 firefox 禁用的功能

```
FirefoxProfile profile = new FirefoxProfile();
profile.setEnableNativeEvents(true);
WebDriver driver = new FirefoxDriver(profile); 
```

### 临时指定插件

有时需要临时让启动的 firefox 带一个插件，如 firebug,来定位问题等。首先要下载这个插件的 xpi 安装包。剩下的就让 selenium webdriver 来完成，如下：

```
FirefoxProfile firefoxProfile = new FirefoxProfile();
firefoxProfile.addExtension(file);
//避免启动画面
firefoxProfile.setPreference("extensions.firebug.currentVersion", "1.10.1"); 
WebDriver driver = new FirefoxDriver(firefoxProfile); 
```