# Cookies

## Cookies

```
// 增加一个 name = "name",value="value"的 cookie
Cookie cookie = new Cookie("name", "value");
dr.manage().addCookie(cookie);

// 得到当前页面下所有的 cookies，并且输出它们的所在域、name、value、有效日期和路径
Set<Cookie> cookies = dr.manage().getCookies();
System.out.println(String
  .format("Domain -> name -> value -> expiry -> path"));
for (Cookie c : cookies)
System.out.println(String.format("%s -> %s -> %s -> %s -> %s",
c.getDomain(), c.getName(), c.getValue(), c.getExpiry(),c.getPath()));

// 删除 cookie 有三种方法

// 第一种 通过 cookie 的 name
dr.manage().deleteCookieNamed("CookieName");

// 第二种 通过 Cookie 对象
dr.manage().deleteCookie(cookie);

// 第三种 全部删除
dr.manage().deleteAllCookies(); 
```