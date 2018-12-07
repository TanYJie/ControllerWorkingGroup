# Calling Conventions

> #### 响应
##### 状态码：20X
对于正确请求的响应的规范参见具体 API 的规范。

##### 状态码：400
表示请求中的信息有误，返回错误消息与一些辅助信息。

响应中必须包含一个 `message` 字段，值为向用户显示的可读的错误信息。

例：

    { "message": "查找的课程不存在"}

##### 状态码：403 Forbidden
表示当前用户的权限不足以完成请求，返回错误消息。

响应中必须包含一个 message 字段，值为向用户显示的可读的错误信息。

例：

学生无法创建课程

    { "message": "仅老师可创建课程"}

##### 状态码：404 Not Found
表示找不到当前请求中要求的资源，返回错误消息。


例：

删除的课程不存在


    { "message": "班级不存在"}

<br>

> #### 登录与鉴权
采用 JWT (Json Web Token) 管理用户登录状态。

##### 什么是 Json Web Token 及为什么使用Json Web Token
  简单来说，就是你用一些方式向服务器证明你是你，服务器发一个证书证明你是你，以后每次服务器需要知道你是谁的时候就出示你是你的证书就好了。 JWT 就是这样一个证书。
  
  具体可以参考下面的博客
> http://www.jianshu.com/p/576dbf44b2ae

##### Json Web Token 规约
###### JWT Header
本应用的 JWT 头统一采用

    { "alg": "HS256", "typ": "JWT" }

这意味着 JWT 签名算法统一为 `HMAC SHA256`。

###### JWT Payload
JWT Payload 中必须包含用户ID `id`、 用户类型 `role` 和过期时间 `exp` 三个字段。具体实现可以添加其他需要的字段，但禁止包含可能影响系统安全或用户数据安全的未加密信息，如用户的密码等。

例:

基本的 Payload
```
{
	"id": "2757",
	"role": "student",
	"exp": 1511700699
}
```
添加了名称字段的 Payload
```
{
	"id": "2757",
	"role": "student",
	"name": "谭源杰",
	"exp": 1511700699
}
```

###### JWT Signature
使用 Base64 后的 Header 与 Payload 和服务端密钥组合并使用 HMAC SHA256 签名后得出。
