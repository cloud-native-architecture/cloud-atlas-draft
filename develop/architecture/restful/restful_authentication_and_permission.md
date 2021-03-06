由于RESTful风格的服务是无状态的，认证机制尤为重要。

认证机制解决的问题是，确定访问资源的用户是谁；权限机制解决的问题是，确定用户是否被许可使用、修改、删除或创建资源。权限机制通常与服务的业务逻辑绑定，因此权限机制需要在每个系统内部定制，而认证机制基本上是通用的，常用的认证机制包括 `session auth`(即通过用户名密码登录)，`basic auth`，`token auth`和`OAuth`，服务开发中常用的认证机制为后三者。

# Basic Auth

asic Auth是配合RESTful API 使用的最简单的认证方式，只需提供用户名密码即可，但由于有把用户名密码暴露给第三方客户端的风险，在生产环境下被使用的越来越少。因此，在开发对外开放的RESTful API时，尽量避免采用Basic Auth。

# Token Auth

Token Auth并不常用，它与Basic Auth的区别是，不将用户名和密码发送给服务器做用户认证，而是向服务器发送一个事先在服务器端生成的token来做认证。因此Token Auth要求服务器端要具备一套完整的Token创建和管理机制，该机制的实现会增加大量且非必须的服务器端开发工作，也不见得这套机制足够安全和通用，因此Token Auth用的并不多。

# OAuth

OAuth（开放授权）是一个开放的授权标准，允许用户让第三方应用访问该用户在某一web服务上存储的私密的资源（如照片，视频，联系人列表），而无需将用户名和密码提供给第三方应用。

OAuth允许用户提供一个令牌，而不是用户名和密码来访问他们存放在特定服务提供者的数据。每一个令牌授权一个特定的第三方系统（例如，视频编辑网站)在特定的时段（例如，接下来的2小时内）内访问特定的资源（例如仅仅是某一相册中的视频）。这样，OAuth让用户可以授权第三方网站访问他们存储在另外服务提供者的某些特定信息，而非所有内容。

正是由于OAUTH的严谨性和安全性，现在OAUTH已成为RESTful架构风格中最常用的认证机制，和RESTful架构风格一起，成为企业级服务的标配。

目前OAuth已经从OAuth1.0发展到OAuth2.0，但这二者并非平滑过渡升级，OAuth2.0在保证安全性的前提下大大减少了客户端开发的复杂性，因此，建议在实战应用中采用OAuth2.0认证机制。

# 参考

* [RESTful 架构风格概述](https://blog.igevin.info/posts/restful-architecture-in-general/)