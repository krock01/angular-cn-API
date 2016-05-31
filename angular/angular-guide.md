# angular中文翻译
## 内容简介
欢迎访问angularjs 中文翻译，此文件针对1.5.5版本

这篇文章主要介绍modules，其中包含了[directives](#derectives),[services](#services),[filters](#filters),[providers](#providers),[templates](#templates),全局APIs和模拟测试.

> Angular的前缀采用 $ 和 $$,$是angular对外的命名前缀,而$$angualr私有的命名前缀,为了不引起命名冲突，请在你的代码中不要使用$或者$$的前缀命名


## Angular Modules


### ng(核心模块)
此模块包含AngularJS核心组件,Angular默认提供


| 名称 | 描述 |
| ---- | ------ |
| [directives](#derectives) | 你可以在构建自己的angular引用此核心指令集合，<br>例如：[ngClick](#ngClick),[ngInclude](#ngInclude),[ngRepeat](#ngRepeat),等等.. |
 [Services/Factories](#Services/Factories) | 你可以通过依赖注入此核心服务集合到你的应用，<br>例如:[$compile](#$compile),[$http](#$http),[$location](#$location),等等|
| [Filters](#Filters) | 过滤器在表达式和指令中使用，过滤器在ngmodule中通过表达式和指令， 使数据在渲染数据到页面之前处理数据. 例如:[filter](#filter),[date](#date),[currency](#currency),[lowercase](#lowercase),[uppercase](#uppercase),等等|
| [Global APIs](#Global APIs) | 全局api方法都依附在angualr对象上，这些方法用于处理低版本的js操作时使 用 |

### ngRoute
ngRoute通过hashbang和 html5 pushState 来路由你的应用

>这个模块需要引用angular-router.js文件，并在你的应用中注入依赖

| 名称 | 描述 |
| ----- | ------- |
| [Services/Factories](#Services/Factories) | 以下服务被应用在路由管理:<ul><li> [$routeParams](#$routeParams)获取url中的查询参数</li></ul><ul><li> [$route](#$route)用来查看正准备进入的route的详情</li></ul><ul><li> [$routeProvider](#$routeProvider)用来注册路由</li></ul>|
| [directives](#derectives) | 使用[ngView](#ngView)指令显示路由加载的页面 |

### ngAnimate
用来给你的应用增加动画效果,当你引用了ngAnimate，大多核心指令提供了动画钩子，动画是用css transition/animations或者js回调实现的
>这个模块需要引用angular-animate.js文件，并在你的应用中注入依赖

| 名称 | 描述 |
| ----- | ------- |
| [Services/Factories](#Services/Factories) | 在你的指令中使用[$animate](#$animate)出发动画操作 |
| [CSS-based animations](#CSS-basedanimations) | 在angularjs中按照ngAnimate's的命名规范引入CSS&nbsp;transition s/keyframe动画,一次定义后即可在引用此样式的模版代码中触发 |
| [JS-based animations](#JS-based animations) | 使用[module.animation()](#module.animation())]注册js动画，一次注册后即可在引用此样式 的模版代码中触发 |

### ngAria
使用ngAria 可以在指令中注入常用的辅助功能，提高残疾人的用户体验
>这个模块需要引用angular-aria.js文件，并在你的应用中注入依赖

| 名称 | 描述 |
| ----- | ------- |
| [Services](#Services) | [$aria](#$aria)提供调用ARIA属性的方法<br>[$ariaProvider](#$ariaProvider)用来配置ARIA属性 |

＃ngResource
引入ngResource模块，可以使用REST API查询数据和提交数据
>这个模块需要引用angular-resource.js文件，并在你的应用中注入依赖

| 名称 | 描述 |
| ----- | ------- |
| [Services/Factories](#Services/Factories) | [$resource](#$resource)服务是用来沟通REST API的对象 |

### ngCookies
使用ngCookies操控cookie
>这个模块需要引用angular-cookies.js文件，并在你的应用中注入依赖

| 名称 | 描述 |
| ----- | ------- |
| [Services/Factories](#Services/Factories) | 以下服务用户cookie管理: <ul><li>[$cookie](#$cookie)服务是一个简便的封装，是用来向浏览器的cookie中存 取简单数据的</li></ul> <ul><li>[$cookieStore](#$cookieStore)是用来存取序列化后的复杂数据的</li></ul> |

### ngTouch
ngTouch 用户支持手机端浏览器/设备
>这个模块需要引用angular-touch.js文件，并在你的应用中注入依赖

| 名称 | 描述 |
| ----- | ------- |
| [Services/Factories](#Services/Factories) | [$swipe](#$swipe)服务用来注册和管理手机dom事件的 |
| [Directives](#Directives) | 各种指令都可以通过naTouch模拟手机dom事件 |

### ngSanitize
使用naSanitize 可以安全的对html 数据进行分析和操作
>这个模块需要引用angular-sanitize.js文件，并在你的应用中注入依赖

| 名称 | 描述 |
| ----- | ------- |
| [Services/Factories](#sanitize/Factories) | [$sanitize](#$sanitize)是用来清理危险的html代码即快速又便捷的方式 |
| [Directives](#Directives) | [$linky filter](#$linky filter)用来把字符串中的urls转换为html链接 |

### ngMock
ngMock在你的单元测试中注入并模拟模块，如:工厂类，服务类和供给类
>这个模块需要引用angular-mocks.js文件到你的test中


| 名称 | 描述 |
| ---- | ------ |
|[Services/Factories](#Services/Factories) | ngMock将扩展许多核心服务成为可以同步测试和管理的服务 例如:[$timeout](#$timeout),[$internal](#$internal),[$log](#$log),[$httpBackend](#httpBackend)等等|
| [Global APIs](#Global APIs) | 各种辅助功能都可以注入并在单元测试代码模拟模块。 例如:[inject()](#inject),[module()](#module), [dump()](#dump),等等 |

