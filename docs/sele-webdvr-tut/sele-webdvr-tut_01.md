# Selenium Webdriver 简易教程

Selenium 是 ThroughtWorks 公司开发的一套 Web 自动化测试工具。

它分为三个组件：

*   Selenium IDE
*   Selenium RC (Remote Control)
*   Selenium Webdriver

**Selenium IDE**是 firefox 的一个插件，允许测试人员录制脚本并回放。

**Selenium RC**和**Selenium Webdriver**是测试框架，提供多种语言的 API。不同的是，**Selenium Webdriver**以一种更底层、更灵活的方式来操作浏览器，并不仅仅使用 javascript。这样它可以绕开浏览器的沙箱限制，实现**Selenium RC**不支持的框架、弹出窗口、页面导航、下拉菜单、基于 AJAX 的 UI 元素等控件的操作。以及，**Selenium Webdriver**不需要本地服务器。

Selenium 1.x 版本只包含前两个组件。从 2.0 开始 Webdriver 加入其中。