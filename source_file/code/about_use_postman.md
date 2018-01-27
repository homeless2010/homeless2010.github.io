title: "记录一次奇怪问题"
date: 2018-01-25 09:59:00 +0800
author: me
tags:
    - 工具
preview: 记录一次奇怪问题。

---

### 奇怪的问题

同事电脑上出现唯独一个接口若在域名后跟连续两个//则后台获取不到登陆信息，百思不得其解。
后来发现是Postman在作祟

![](-/images/question1.png)

若打开右上角Postman Interceptor则chrome会拦截请求并同步到Postman

![](-/images/question2.png)

然后用Postman发起该请求，服务器响应Cookie有两个path

![](-/images/question3.png)

再次用chrome发起该请求发现两Cookie，等到其中一个（应该是path不为根的那个）失效后就会发现该接口获取不到当前登录人而明明已登陆系统。

![](-/images/question4.png)
