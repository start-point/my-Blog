---
title: git push 出现的问题
---

每次登录账号密码都会推送失败,会出现同下这个问题,
![在这里插入图片描述](https://img-blog.csdnimg.cn/3b0b6c9b72194ab595b1d56ad185e5e4.png#pic_left)

```git
Logon failed, use ctrl+c to cancel basic credential prompt.
Username for 'https://github.com': echo_c120@163.com
remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information.
fatal: unable to access 'https://github.com/start-point/webpack.git/': The requested URL returned error: 403
//登录失败，使用 ctrl+c 取消基本凭据提示。
//“https://github.com”的用户名：echo_c120@163.com
//远程：2021 年 8 月 13 日移除了对密码身份验证的支持。请改用个人访问令牌。
//远程：请参阅 https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ 了解更多信息。
//致命：无法访问“https://github.com/start-point/webpack.git/”：请求的 URL 返回错误：403
```

该问题是需要你升级git 去github设置一个个人访问令牌,

解决办法,先去https://gitforwindows.org/官网下载最新版的git

接着进去自己的github官网

点击Settings



点击Developer settings



再去点击Personal access tokens



点击新建一个token



然后保存记住会生成一个token 记住它



