---
layout: post
title: json-web-tokens-vs-session-cookies
---

| Cookies        | JWT           |
| ------------- |:-------------:| 
| 通常比较小(4k hard limit)     | 可以在token中存大量内容(8k soft limit) | 
| 每次同域请求都带在头部      | 可以只在需要时发      | 
| 可以验证静态文件 | 使用签名请求，麻烦点      | 
| cookie独特的存储机制 | 只能存在LocalStorage或SessionStorage      | 
| 很难跨域使用 | 可以在任何域使用      |
| 可以被多个子域访问 | localStorage只能被所存储的子域访问      |  
| 如果用了application/json会触发飞行前请求 | 需要飞行请求      |  
| 受CSRF(跨站请求伪造)攻击 | 不受CSRF攻击      | 
| HttpOnly让[XSS](https://www.owasp.org/index.php/Cross-site_Scripting_(XSS))很难实行 | 不需要保护XSS      |  
| Secure flag forces SSL -- Man in the Middle(MITM) | 应用内管理SSL      |  
| 从浏览器中获取cookie非常简单 | LocalStorage也非常容易从浏览器中获取      |  
| 移动端支持不多 | 移动端标准认证   |
| 必须由服务器设置 | 可以被任何人修改   | 
| 不能用在外部api请求 | 可以被作为公共api使用   |  
| 包涵一个session id | 包涵验证用户信息   |  
| 需要一个数据库检查每一个请求 | 不需要数据库检查请求   |  
| Server-side sessions require subsequent requests to hit same server | 状态被存在客户端   |  
| 难以扩展 | 容易扩展   |  


### 参考
https://ponyfoo.com/articles/json-web-tokens-vs-session-cookies
https://www.slideshare.net/derekperkins/authentication-cookies-vs-jwts-and-why-youre-doing-it-wrong


