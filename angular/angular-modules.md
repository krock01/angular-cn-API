### ng
AngularJS 应用启动时默认加载ng module,该模块本身包含一个AngularJS应用功能重要组成部分。下表列出的services/factories, filters, directives and testing components都可以在此核心模块中使用获得

### 已知的错误

| 名称 | 描述 |
| --- | ---- |
| [angular.toJson](#angular.toJson) | 当他转换错误的日期格式时会在safari浏览器使用时会抛出一个<br>`RangeError`而不是null,可以通过使用下面这种途径修复<br>`var _DatetoJSON = Date.prototype.toJSON;Date.prototype.toJSON = function(){try{ return _DatetoJSON.call(this);}catch(e){if(e instanceof RangeError){return null;}throw e;}}`|
| [angular.element](#angular.element) | 如果你使用 1.x Jasmine将不能监听`angular.element`，查看更多 [https://github.com/angular/angular.js/issues/14251](https://github.com/angular/angular.js/issues/14251) |
| [$interpolate](#$interpolate) | 在使用多个`{{}}`链接时应该特别注意，不然会造成不能正确解析的错误,例如`<data-context='{"context":{"id":3,"type":"page"}}'>` 上面这行代码就会出现错误处理问题,解决办法是增加空格分隔,`data-context='{"context":{"id":3,"type":"page"} }"` |

### Modules Components
#### Function
| 名称 | 描述 |
| --- | ---- |
| [angular.forEach](#angular.forEach) | 调用迭代函数`iterator`便利对象集合的每一个,这个集合<br>对象可以是对象也可以是数组这个`iterator`函数这么调<br>用的`iterator(value,key,obj)`,`value`是一个对象属性<br>或者数组元素,`key`是一个对象的key或者是一个数组元<br>素的下标序号,`obj`是对象本身，给定的函数上下文是可<br>选的 |
| [angular.extend](#angular.extend) | 枚举`src`对象的所有属性复制到目标对象`dst`,你可以指<br>定多个`src`对象，如果你想保护原有对象，你可以穿入<br>一个空的对象作为目标 |
| [angular.merge](#angular.merge) | 意思同上;只是angular api中说是更深层次的扩展 |
| [angular.noop](#angular.noop) | 不执行任何操作的函数,在编写函数风格时会有用<br>`function foo(callback){`<br>`var result = calculateResult();` <br>`(callback  ||  angular.noop)(result);`<br> `}` |
| [angular.identity](#angular.identity) | 此函数返回其第一个参数，在编写函数风格时会有用 |
| [angular.isUndefined](#angular.isUndefined) | 是否为undefined |
| [angular.isDefined](#angular.isDefined) | 是否为defined |
| [angular.isObject](#angular.isObject) | 是否为一个对象<br>不同于js的`typeof null`不被看作对象,<br>注:arrays是一个对象 |
| [angular.isString](#angular.isString) | 是否是一个`string` |
| [angular.isNumber](#angular.isNumber) | 是否是一个`number` |
| [angular.isDate](#angular.isDate) | 是否是一个日期`date` |
| [angular.isArray](#angular.isArray) | 是否为一个`Array` |
| [angular.isFunction](#angular.isFunction) | 是否为一个`Function` |
| [angular.isElement](#angular.isElement) | 是否为dom元素（或者被jquery封装的元素） |
| [angular.copy](#angular.copy) | 创建一个`source`的深层复制,这个`source`可以是对象也<br>可以是数组 |
| [angular.equals](#angular.equals) | 确认两个对象或者值是否相等，值所支持的类型有:表达<br>式,数组和对象  |
| [angular.bind](#angular.bind) | 返回一个绑定`self`(`self`作为`fn`的`this`对象)的函数<br>`fn`.你可以任意提供准备绑定此函数的参数.这里应该了<br>解[partial application](#局部应用)和[function currying](#局部套用)的区别 |
| [angular.toJson](#angular.toJson) | 序列化json格式的字符串,自从angualr内部使用了$$标记<br>后，以$$开头的属性会被排除在外 |
| [angular.fromJson](#angular.fromJson) | json字符串反序列化为json对象 |
| [angular.bootstrap](#angular.bootstrap) | 使用此函数手动启动angular应用 |
| [angular.reloadWithDebugInfo](#angular.reloadWithDebugInfo) | 使用此函数来重启当前已开启调试信息的应用,并且优先<br>级高于`$complieProvider.debugInfoEnabled(false)`. |
| [angular.injector](#angular.injector) | 创建一个可以用来检索服务和依赖注入的对象<br>(查看[依赖注入](#dependency injection)) |
| [angular.element](#angular.element) | 封装一个原始的dom元素或者html片段为jquery元素 |
| [angular.module](#angular.module) | `angular.module`可以全局创建、注册和检索Angular模块<br>,全部模块(核心模块和第三方模块)想要在一个应用中使用<br>，必须使用此机制注册 |

### Directive
| 名称 | 描述 |
| --- | ---- |
| [ngJq](#ngJq) | 使用此指令强制指定`angular.element`所使用的库,如果`ng-jq`值为空<br>将使用jqlite,如果有名字，则设置为window下jquery变量的名称(例如:<br>jquery) |
| [ngApp](#ngApp) | 使用此指令自动启动AngularJS应用,`ngApp`指令可以指定应用的根节<br>点，常常在页面根元素的附近-例如:在`body`或者`html`标签上 |
| [a](#a) | 修改了它在html中a标签本身的行为，在`href`属性为空时，阻止了`a`<br>默认动作 |
| [ngHref](#ngHref) | 使用此指令时为了解决href属性中`{{hash}}`在未被angualr替换为具<br>体值时出现的路径错误 |
| [ngSrc](#ngSrc) | 理由同上 |
| [ngSrcset](#ngSrcset) | 理由同上 |
| [ngDisabled](#ngDisabled) | 此指令会设置html标签中的`disabled`属性.如果值是表达式那么<br>angualr会计算是否为true |
| [ngChecked](#ngChecked) | 设置html标签中的`checked`属性的值，同样可以使用表达式 |
| [ngReadonly](#ngReadonly) | 设置html标签中的`readOnly`属性的值,同样可以使用表达式 |
| [ngSelected](#ngSelected) | 设置html标签中的`selected`属性的值,同样可以使用表达式 |
| [ngOpen](#ngOpen) | 设置html标签中的`open`属性的值,同样可以使用表达式 |
| [ngForm](#ngForm) | form指令可嵌套的别名，html本身是不支持嵌套的.例如:如果要检查<br>一个子组件控制的有效性 |
| [form](#form) | 在使用此指令时会初始化[FormController](#FormController) |
| [textarea](#textarea) | 此指令给html `textarea`标签增加了angular的数据绑定，这个数据绑<br>定和验证属性类似于[input element](#input element) |
| [input](#input) | 此标签配合`ngModel`使用，会提供数据绑定,输入状态控制和验证，输<br>入控制遵循html5的类型和针对旧浏览器的html5的验证行为的<br>`polyfills` |
| [ngValue](#ngValue) | 此指令用语html标签`<option>`或者`input[radio]`，当选中元素时,<br>`ngModel`将设置绑定的值 |
| [ngBind](#ngBind) | `ngbind` 属性通知angular根据指定的元素的表达式值去替换内容，当<br>表达式值改变时也会更新此内容 |
| [ngBindTemplate](#ngBindTemplate) | `ngBindTemplate`指令是使用插入模版去替换元素文本内容，该指令中<br>可以使用多个`{{}}`表达式 |
| [ngBindHtml](#ngBindHtml) | 计算表达式的值和插入得到的html到元素中会使用一个安全的方式,默<br>认得到的html内容插入时会使用[$sanitize](#$sanitize)过滤不安全因素,要使用此功<br>能，请确保$sanitize方法可用 |
| [ngChange](#ngChange) | 当用户改变输入内容时计算表达式的值,这个表达式的计算是直接执行<br>的,而不像js onchange事件,在change事件(通常当用户离开这个表单<br>元素或者按下任意键)触发之后才执行 |
| [ngClass](#ngClass) | 此指令通过绑定表达式可以让你灵活的设置css classes,这相当于所有<br>的classes都可以背添加 |
| [ngClassOdd](#ngClassOdd) | `ngClassOdd`和`ngClassEven`这两个指令工作原理就是[ngClass](#ngClass)可是他<br>们联合`ngRepeat`使用，并只影响奇(偶)行 |
| [ngClassEven](#ngClassEven) | `ngClassOdd`和`ngClassEven`这两个指令工作原理就是[ngClass](#ngClass)可是他<br>们联合`ngRepeat`使用，并只影响奇(偶)行 |
| [ngCloak](#ngCloak) | `ngCloak`指令为了避免angular模版在你的应用未加载完成出现的问题,<br>使用这个指令以避免造成HTML模板显示不良闪烁效果。 |
| [ngController](#ngController) | `ngController`指令为视图附加一个控制器类,这是angualr支持mvc思<br>想的一个重要组成部分 |
| [ngCsp](#ngCsp) | angular 有一些可以打破CSP(内容安全策略)规则 |
| [ngClick](#ngClick) | 单击事件 |
| [ngDblClick](#ngDblClick) | 双击事件 |
| [ngMousedown](#ngMousedown) | 鼠标点击事件 |
| [ngMouseup](#ngMouseup) | 鼠标松开事件 |
| [ogMouseover](#ogMouseover) | 鼠标悬浮事件 |
| [ngMouseenter](#ngMouseenter) | 鼠标进入事件 |
| [ngMouseleave](#ngMouseleave) | 鼠标离开事件 |
| [ngMousemove](#ngMousemove) | 鼠标移动事件 |
| [ngKeydown](#ngKeydown) | 按键按下事件 |
| [ngKeyup](#ngKeyup) | 按键松开事件 |
| [ngSubmit](#ngSubmit) | 可以绑定angular表达式到提交事件上 |
| [ngFocus](#ngFocus) | 获取焦点事件 |
| [ngBlur](#ngBlur) | 失去焦点事件 |
| [ngCopy](#ngCopy) | 复制事件 |
| [ngCut](#ngCut) | 剪切事件 |
| [ngPaste](#ngPaste) | 粘贴事件 |
| [ngIf](#ngCut) | `ngIf`指令根据表达式的值来移除或者创建部分dom，如果表达式的<br>值为false将移除此dom元素，否则就重新插入这个元素到dom中 |
| [ngInclude](#ngInclude) | 引用外部html片段 |
| [ngInit](#ngInit) | ngInit指令允许你在当前作用域计算表达式的值 |
| [ngList](#ngList) | 文本输入的带分隔符的字符串可以转换为数组，默认分隔符为`, `就像<br>这样`ng-list=", "`,你可以自定分隔符 例如：`ng-list="|"` |
| [ngModel](#ngModel) | 该指令把`input`,`select`,`textarea`(或者常用的表单管理)作为一个属<br>性绑定在`NgModelController`的作用域上,`NgModelController`可以凭<br>借这个指令来创建和暴露|
| [ngModelOptions](#ngModelOptions) | 允许更改数据更新的方式,你可以在常用的事件列表中 定义其中一个<br>事件,它将会在数据更新或者延迟后触发,事实上在世间到期后更新操<br>作会发生，另一个更改发生时，计时器会重置 |
| [ngNonBindable](#ngNonBindable) | 该指令是让angular不编译和绑定内容到当前dom元素。如果你想要<br>显示一段绑定或者指定代码时会很有用,比如：你有一个网站需要展<br>示代码片段 |
| [ngOptions](#ngOptions) | `ngOptions`理解表达式可以获取到数组或者对象对`<select>`动态生<br>成`<option>`列表 |
| [ngPluralize](#ngPluralize) | `ngPluralize`是根据en-US本地化规则显示消息的指令.这些规则绑<br>定在angular.js，但可以覆盖（见角的[i18n开发者指南](#Angular i18n)）.通过指定多<br>个类别和字符串之间的映射显示配置ngPluralize指令。 |
| [ngRepeat](#ngRepeat) | 循环遍历集合中的每个项,每个项都有自己的作用域,循环变量设置为<br>当前项的数组对象，`$index`设置为当前的索引或者key值 |
| [ngShow](#ngShow) | 该指令通过表达式来显示或者隐藏对应的html元素,元素的显示和隐藏<br>时通过删除和天价`.ng-hide`央视来处理的，这里的`.ng-hide`时<br>angularjs预先定义其样式中的display属性为none(using an !important<br> flag),针对csp模式，应添加`angular-csp.css`文件到你的html文件中<br>(查看[ngCsp](#gCsp)) |
| [ngHide](#ngHide) | 该指令通过表达式来显示或者隐藏对应的html元素,元素的显示和隐藏<br>时通过删除和天价`.ng-hide`央视来处理的,这里的`.ng-hide`时<br>angularjs预先定义其样式中的display属性为none(using an !important <br>flag),针对csp模式，应添加`angular-csp.css`文件到你的html文件中<br>查看[ngCsp](#gCsp) |
| [ngStyle](#ngStyle) | 该指令可以根据条件设置html元素的样式 |
| [ngSwitch](#ngSwitch) | 基于一个作用域内的表达式`ngSwitch`可以根据条件改变dom结构,包<br>含`ngSwitch`的元素却不包含`ngSwitchWhen`或者`ngSwitchDefault`指<br>令时，将保留这个元素在模版中指定的位置 |
| [ngTransclude](#ngTransclude) | 标记被嵌入dom插入最近使用嵌入的父指令的位置 |
| [script](#script) | 加载`<script>`的内容到`$templateCache`,可以被`ngInclude`,`ngView`,<br>或[directive](#directive).这种`script`元素必须指定类型为`text/ng-template`,指定<br>`id`，可以在指令的`templateUrl`中调用 |
| [select](#select) | html 的`SELECT`增加了angular的数据绑定 |
| [ngRequired](#ngRequired) | 该指令配合`ngModel`和`validator`验证非空.常常用于`inpput`,`select`<br>控件，但也可以用在常用控件上 |
| [ngPattern](#ngPattern) | 该指令配合`ngModel`和`validator`使用正则验证.常常用于`inpput`控<br>件，但也可以用在常用文本控件上 |
| [ngMaxlength](#ngMaxlength) | 该指令配合`ngModel`和`validator`验证最大长度.常常用于文本控件,<br>但也可以用在常用文本控件上 |
| [ngMinlength](#ngMinlength) | 该指令配合`ngModel`和`validator`验证最小长度.常常用于文本控件,<br>但也可以用在常用文本控件上 |

###Object
| 名称 | 描述 |
| --- | ---- |
| [angular.version](#angular.version) | 该对象包含当前版本号 |

### Type
| 名称 | 描述 |
| --- | ---- |
| [angular.Modules](#angular.Modules) | 配置angular modules接口 |
| [$cacheFactory.Cache](#$cacheFactory.Cache) | 是一个可以存取数据的缓存对象，主要作用在[$http](#$http)和<br>[script](#script)指令上，用来存储模版和一些数据 |
| [$compile.directive.Attributes](#$compile.directive.Attributes) | 一个在compile/linking函数之间共享的对象,其中包含正常<br>的dom元素的属性,这个值反应的是当前绑定的状态`{{}}`<br>.正常化的元素是需要的，因为这些都等同于angular在<br>处理 |
| [form.FormController](#form.FormController) | `FormController`一直在追踪所有的控件和嵌套的form以<br>及它们的状态,例如 valid/invalid或者dirty/pristine |
| [ngModel.NgModelController](#ngModel.NgModelController) | `NgModelController`对`ngModel`指令提供的.这个控制器包<br>含了数据绑定，验证，css更新和格式化.它故意不包含它<br>与DOM渲染或听DOM事件涉及的任何逻辑,这些与dom<br>相关的逻辑应该由其它使用数据绑定到模版的指令来实<br>现.angular为打多输入元素提供了这部分逻辑,在这个页<br>面的最后部分你能找到使用`ngModelController`绑定可编<br>辑元素的[常用控件事例](#custom control example) |
| [select.SelectController](#select.SelectController) | 真对`select`指令的控制器,提供了读写选中项和协调<br>动态添加`<option>`(或者用`ngRepeat`)的值的功能 |
| [$rootScope.Scope](#$rootScope.Scope) | 根作用域可以使用从[$injector](#$injector)取出的[$rootScope](#$rootScope)进行检索,<br>子作用域使用[$new()](#$new())创建(多数作用域都是当html模版被编<br>译时执行自动创建)查看[Scopes guide](#Scopes guide)深入介绍和用法示例 |

### Provider

| 名称 | 描述 |
| --- | ---- |
| [$anchorScrollProvider](#$anchorScrollProvider) | 只要[$location.hash()](#$location.hash())改变，都会禁止默认滚动 |
| [$animateProvider](#$animateProvider) | `$animate`不执行任何动画的默认实现,代替的是同步执行<br>dom更新和resolves the returned runner promise |
| [$comoileProvider](#$comoileProvider) | 原文中未解释 |
| [$controllerProvider](#$controllerProvider) | [$controller service](#$controller service)用来创建新的控制器 |
| [$filterProvider](#$filterProvider) | 过滤器是一个改变输入内容作为输出结果的一个函数,然而<br>过滤器需要被依赖注入,这个过滤器的定义由一个工厂函数<br>组成,这个函数标注了依赖关系和负责创建过滤函数 |
| [$httProvider](#$httProvider) | 使用`$httpProvider`改变[$http](#$http)的默认行为 |
| [$interpolateProvider](#$interpolateProvider) | 作为配置的插入标记,默认为`{{` 和`}}` |
| [$locationProvider](#$locationProvider) | 使用`$locationProvider`配置应用的深度链接路径如何存储 |
| [$logProvider](#$logProvider) | `$logProvider`用来配置应用的如何记录日志的 |
| [$parseProvider](#$parseProvider) | `$parseProvider`配置[$parse](#$parse)服务的默认行为 |
| [$rootScopeProvider](#$rootScopeProvider) | 提供$rootScope服务 |
| [$sceDelegateProvider](#$sceDelegateProvider) | `$sceDelegateProvider`允许开发者们配置[$sceDelegate](#$sceDelegate)<br>服务.也允许获取和设置白名单和黑名单以确保来源于<br>angular的模版安全.<br>涉及到[$sceDelegateProvider.resourceUrlWhitelist](#$sceDelegateProvider.resourceUrlWhitelist) 和 [$sceDelegateProvider.resourceUrlBlacklist](#$sceDelegateProvider.resourceUrlBlacklist)|
| [$sceProvider](#$sceProvider) | `$sceProvider`允许开发者们配置[$sce](#$sce)服务<br><ul><li>在模块中开启或者关闭严格模式</li><li>覆盖自定义委托的实现</li></ul> |
| [$templateRequestProvider](#$templateRequestProvider) | 配置当发出模版请求时传递给`$http`服务的选项 |

### Service

| 名称 | 描述 |
| --- | ---- |
| [$anchorScroll](#$anchorScroll) | 调用这个服务可以滚动到指定的哈希元素或者滚动到<br>[$location.hash()](#$location.hash())当前值所在的位置,根据HTML5规范中指<br>定的规则 |
| [$animate](#$animate) | 该服务公开了一些实用的提供支持动画勾子的dom方法,应<br>用的默认行为是dom操作，然而当你开启了动画,`$animate`<br>将会去做这些繁重的工作以确保动画和触发dom操作的运<br>行 |
| [$animateCss](#$animateCss) | 这是`$animateCss`的核心版本,默认情况下,只有当<br>`ngAnimate`被引入时,`$animateCss`服务才能运行动画 |
| [$cacheFactory](#$cacheFactory) | 工厂创建[Cache](#Cache)对象并提供对它的访问 |
| [$templateCache](#$templateCache) | 在第一次使用一个模板时,它被装入模板缓存进行快速检索.<br>你可以使用script标签直接加载模版到缓存,或直接消耗<br>$templateCache服务 |
| [$compile](#$compile) | 编译一个html字符串和dom到一个模版,并产生一个模版的<br>函数,那么它可以把scope和模版链接到一起 |
| [$controller](#$controller) | `$controller`服务负责实例化控制器 |
| [$document](#$document) | 这是`JQuery or jqLite`对浏览器本身`window.docuemnt`<br>对象的封装 |
| [$exceptionHandler](#$exceptionHandler) | 这个服务实现了任何angualr表达式中出现的未捕获异常的<br>委托.默认的简单实现是,使用`$log.error`在浏览器控制台<br>打印 |
| [$filter](#$filter) | 过滤器用来格式化和控制对用户的数据展示 |
| [$httpParamSerializer](#$httpParamSerializer) | 把`$http`的参数序列化,根据以下规则把对象转换为字符串<br>([查看规则点击](#$httpParamSerializer)) |
| [$httpParamSerializerJQLike](#$httpParamSerializerJQLike) | 用Jquery的param()方法代替`$http`的参数序列化，此序列<br>化会对参数按字母排序 |
| [$http](#$http) | `$http`服务是angular js的核心服务，它是通过浏览器的<br>XMLHttpRequest或通过JSONP沟通远程服务器的 |
| [$xhrFactory](#$xhrFactory) | 用来创建XMLHttpRequest对象的工厂方法 |
| [$httpBackend](#$httpBackend) | HTTP后端被委托向XMLHttpRequest对象或者JSONP<br>的[服务](#$http)使用和处理浏览器兼容性 |
| [$interpolate](#$interpolate) | 编译一个有标记的字符串到插入函数中，这个服务被html <br>[$compile](#$compile)用来数据绑定.见[$interpolateProvider](#$interpolateProvider)用于配置插<br>标记. |
| [$interval](#$interval) | Angular封装了`window.setInterval`.该Fn功能每毫秒的延<br>迟执行. |
| [$locale](#$locale) | $locale服务提供了真对许多angular组件的本地化规则,只有<br>一个[公共方法](#$locale) |
| [$location](#$location) | $location服务对浏览器地址栏进行分析(基于<br>[window.location](#window.location))和从你的应用获取url,改变url会映射到<br>$location服务，改变$location也会映射到浏览器的地址<br>栏 |
| [$log](#$log) | 一个简单的日志服务,默认实现安全的写入信息到浏览器<br>的控制台 |
| [$parse](#$parse) | 转换angular的表达式成为一个函数 |
| [$q](#$q) | 这个服务帮助你异步的运行函数,当它们运行完毕返回一些<br>值(或者异常) |
| [$rootElement](#$rootElement) | Angular应用的根元素,这个元素是[ngApp](#ngApp)生命的位置或者<br>通过`angular.bootstrap`启动，这个元素相当于应用的<br>根节点,它也是应用程序的注射器服务被发布的位置，并<br>且可以使用$rootElement.injector()获取 |
| [$rootScope](#$rootScope) | 每一个应用都有一个独立的根作用域,所有的其它作用域都<br>是根作用域的后代,作用域提供了模型与视图的分离，通过<br>监听模型的改变的机制，它们也提供了事件的发射/广播及<br>认购设施,见[developer guide on scopes](#developer guide on scopes)|
| [$sceDelegate](#$sceDelegate) | 为Angular js提供严格模式 |
| [$templateRequest](#$templateRequest) | `$templateRequest`服务提供安全检查,然后通过`$http`下<br>载模版,成功后存储在`templateCache`里,如果http请求失败<br>或响应为空,编译错误将会被抛出(这个异常可以使用设置<br>第二个参数为true来避免),$templateCache的内容是可信<br>的，如果tpl是字符串类型和$ templateCache有匹配项,可<br>以省略到使用$sce.getTrustedUrl(tpl) |
| [$timeout](#$timeout) | angular对`window.setTimeout`的封装,`fn`函数被封装为<br>`try/catch`块和委托任何一个异常到[$exceptionHandler](#$exceptionHandler)服<br>务|
| [$window](#$window) | 是一个对浏览器`window`对象的引用，`window`在js中的全<br>局变量,他会导致可测试性问题,因为它是全局变量.在<br>angular中我们经常引用`$window`服务,所以它可以被覆盖.<br>删除或模拟来测试. |

### input

| 名称 | 描述 |
| --- | ---- |
| [input[text]](#input[text]) | 继承了html input文本的功能,并实现了数据绑定 |
| [input[date]](#input[date]) | 日期输入框有数据验证和转换,在不支持HTML5日期输入框的<br>浏览器,将会使用文本元素代替,在这种情况下,文本必须输入<br>iso-8601的日期格式(yyyy-MM-dd),例如:`2009-01-06`.因为当前许<br>多浏览器都不支持这个输入格式,所以通过placeholder或者label<br>提示用户是非常必要的 |
| [input[datetime-local]](#input[datetime-local]) | 针对时间的验证和转换,在不支持HTML5日期输入框的浏览器,将<br>会使用文本元素代替,在这种情况下,文本必须输入iso-8601的本地<br>时间格式(yyyy-MM-ddTHH:mm:ss),例如:`2009-01-06T14:57:00` |
| [input[time]](#input[time]) | 意思同上，针对时间格式(HH:mm:ss) |
| [input[month]](#input[month]) |意思同上,格式(yyyy-MM) |
| [input[number]](#input[number]) | 只能输入数字，设置不是数字的错误提示 |
| [input[url]](#input[url]) | 验证是否为url格式，设置不是url时的错误提示 |
| [input[email]](#input[email]) | 验证是否为email 设置不是email的错误提示 |
| [input[radio]](#[input[radio]) | html单选按钮 |
| [input[checkbox]](#input[checkbox]) | html复选框 |

### Filter

| 名称 | 描述 |
| --- | ---- |
| [filter](#filter) | 选择一个子集作为一个新的数组 |
| [currency](#currency) | 格式化数字为货币,当没有定义货币符号时，将默认加载本地化的  |
| [number](#number) | 格式化数字为文本 |
| [date](#date) | 根据请求的格式字符串格式化日期 |
| [json](#json) | 允许转换js对象为json字符串 |
| [lowercase](#lowercase) | 转换字符串为小写 |
| [uppercase](#uppercase) | 转换字符串为大写 |
| [limitTo](#limitTo) | 创建一个包含有指定数目的数组或者字符串,这个元素从原数组的开头或者<br>末尾截取,字符串或者数字都根据指定的值和标志截取,如果输入的是一个数<br>字类型,将被转换为字符串 |
| [orderBy](#orderBy) | 根据确定的表达式给指定数组排序,他会按照字母顺序给字符串排序和按照<br>数字顺序给数字排序,注意,如果一个数字没有被正常排序,那么你就需要确认<br>他是否被当作一个数字处理而不是字符串.类数组也是支持的(例如：<br>NodeLists,jQuery objects TypedArrays,Strings等等) |















