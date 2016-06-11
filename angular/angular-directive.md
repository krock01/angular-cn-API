# 创建指令
>note:这片引导是针对对angularjs基础功能已经了解的开发者,如果你仅仅是刚开始学习,请前往[tutorial](#).如果你想查看指令的api,我们已经移动到[$compile](#)

这篇文档讲解的是,当你想在自己的angular App中自定义指令时,应该如何去实现

## 什么是指令
站在一个高层次来说,指令就是在dom元素上的一组标记,Angular的html编译器把一个具体的行为绑定到dom元素上,或者更甚至改变dom元素和其子元素.

Angular附带这一组内置的指令,比如 `ngBind`,`ngModel`,and  `ngClass`.就像你创建控制器和服务一样,你可以创建你自己的指令,当Angular启动你的应用后,HTML compiler将会扫描整个dom去匹配带有指令的dom元素

## 匹配指令
在我们写一个指令之前,我们需要知道angular的HTML compiler如何确定什么时候使用一个指令.

就像一个元素与一个选择器匹配一样,当这个指令是元素声明的一部分时,我们可以说这个元素和这个指令匹配

在下面的示例中,我们说`<input>`元素与`ngModel`指令相匹配
``` html
<input ng-model="foo">
```
下面这个例子也同样匹配
```html
<input data-ng-model="foo">
```
还有下面这种方式 元素匹配`person`指令
```
<person>{{name}}</person>
```

## 规范化

Angular规范化元素的标签和属性的名称，以确定哪些元素匹配该指令。我们通常以区分大小写的驼峰式规范命名的(e.g`ngModel`).然而,因为html不区分大小写,我们在dom中使用小写的形式,通常我们在dom上使用短划线分割属性(e.g `ng-model`).

规范化处理如下:
1. 分离元素或者属性的前面部分`x-`和`data-`
2. 转换`:`,`-`,或`_`分隔符名称为驼峰式

例如,下面表单类似并且匹配`ngBind`指令
```html
<div ng-controller="Controller">
  Hello <input ng-model='name'> <hr/>
  <span ng-bind="name"></span> <br/>
  <span ng:bind="name"></span> <br/>
  <span ng_bind="name"></span> <br/>
  <span data-ng-bind="name"></span> <br/>
  <span x-ng-bind="name"></span> <br/>
</div>
```
## 指令的类型
`$compiler`可以基于元素名称,属性,类名,注释进行指令匹配.
Angular提供的所有指令都可使用属性名,标签名,注释或者类名进行匹配.下面的示例就是多种途径使用指令的
```html
<my-dir></my-dir>
<span my-dir="exp"></span>
<!-- directive: my-dir exp -->
<span class="my-dir: exp;"></span>
```
## 创建指令
首先讨论下指令的注册,就像控制器,指令也需要注册到模块.为了注册指令,你要使用`module.directive`API.`module.directive`以标准化的指令的名称后跟一个工厂函数,这个工厂函数需要返回一个对象,用不同的配置告诉`$compiler`在匹配到此指令时如何运行.

当编译器第一次匹配到该指令时,这个工厂函数只会被调用一次,你可以在这里做任何初始化操作.该功能使用`$injector.invoke`使其注入就像一个控制器一样被调用

我们将重温几个在指令中常见的例子,然后深入学习不同的选项和编译过程

在下面这些例子中,我们将使用前缀`my`(e.g. `myCustomer`)

## 模板扩展指令

假如你有大量的相当于客户信息的模版,此模版在你的代码中重复是哦那个好多次,当你改变一个地方时,你还需要多处进行同样的操作.此时就是一个利用指令简化你的模版的好机会

让我们来创建一个指令,这个指令简单的使用静态模版替换内容

```html
<div ng-controller="Controller">
  <div my-customer></div>
</div>

```
```javascript
angular.module('docsSimpleDirective', [])
.controller('Controller', ['$scope', function($scope) {
  $scope.customer = {
    name: 'Naomi',
    address: '1600 Amphitheatre'
  };
}])
.directive('myCustomer', function() {
  return {
    template: 'Name: {{customer.name}} Address: {{customer.address}}'
  };
});
```
我们绑定了此指令,在`$compile`编译和链接`<div my-customer></div>`,它将试图在该元素的子节点上匹配指令,这也就是说你可以结合多个指令,我们将在下面的例子中看到这是如何做到的.

在上面的例子中我们在模版选项的行内写的值,但是这将随着你模版长度的增加会造成烦恼.

如果你熟悉`ngInclude`,`templateUrl`和他的工作原理相似.下面是同样的一个实例,只是用`templateUrl`替换了`template`:
>index.html
```html
<div ng-controller="Controller">
  <div my-customer></div>
</div>
```

>script.js
```javascript
angular.module('docsTemplateUrlDirective', [])
.controller('Controller', ['$scope', function($scope) {
  $scope.customer = {
    name: 'Naomi',
    address: '1600 Amphitheatre'
  };
}])
.directive('myCustomer', function() {
  return {
    templateUrl: 'my-customer.html'
  };
});
```

>my-customer.html

```html
Name: {{customer.name}} Address: {{customer.address}}
```
`templateUrl`也可以是一个函数,这个函数返回一个被指令使用的模版地址.`templateUrl`有两个参数:指令被调用的元素对象,和元素相关的属性对象

>index.html

```html
<div ng-controller="Controller">
  <div my-customer type="name"></div>
  <div my-customer type="address"></div>
</div>
```

>script.js

```javascript
angular.module('docsTemplateUrlDirective', [])
.controller('Controller', ['$scope', function($scope) {
  $scope.customer = {
    name: 'Naomi',
    address: '1600 Amphitheatre'
  };
}])
.directive('myCustomer', function() {
  return {
    templateUrl: function(elem, attr){
      return 'customer-'+attr.type+'.html';
    }
  };
});
```

>customer-name.html
```html
Name: {{customer.name}}
```

>customer-address.html

```html
Address: {{customer.address}}
```
`restrict`常用选项:
* `A` -只匹配属性名称
* `E` -只匹配元素名称
* `C` -只匹配class名称
* `M` -只匹配注释
这些约束可以联合使用:
* `AEC` -匹配属性,元素,class

让我们来用`restrict:"E"`修改我们的指令

>index.html
```html
<div ng-controller="Controller">
  <div my-customer></div>
</div>
```

>script.js
```javascript
angular.module('docsTemplateUrlDirective', [])
.controller('Controller', ['$scope', function($scope) {
  $scope.customer = {
    name: 'Naomi',
    address: '1600 Amphitheatre'
  };
}])
.directive('myCustomer', function() {
  return {
    restrict: `E`,
    templateUrl: 'my-customer.html'
  };
});
```

>my-customer.html

```html
Name: {{customer.name}} Address: {{customer.address}}
```


用mycustomer指令元素显然是正确的选择，因为你没有一些“客户”行为装饰元素；你定义核心元素的行为作为一个客户组件。

## 隔离指令的Scope

我们的`myCustomer`指令很好,但是他有一个致命的瑕疵.我们只能在给定的scope中使用一次

在当前的实现中,我们需要每次创建一个不同的控制器,以复用这个指令

>script.js

```javascript
angular.module('docsScopeProblemExample', [])
.controller('NaomiController', ['$scope', function($scope) {
  $scope.customer = {
    name: 'Naomi',
    address: '1600 Amphitheatre'
  };
}])
.controller('IgorController', ['$scope', function($scope) {
  $scope.customer = {
    name: 'Igor',
    address: '123 Somewhere'
  };
}])
.directive('myCustomer', function() {
  return {
    restrict: 'E',
    templateUrl: 'my-customer.html'
  };
});
```

>index.html
```html
<div ng-controller="NaomiController">
  <my-customer></my-customer>
</div>
<hr>
<div ng-controller="IgorController">
  <my-customer></my-customer>
</div>
```

>my-customer.html

```html
Name: {{customer.name}} Address: {{customer.address}}
```

这很明显不是一个好的解决方案

我们想要做的是分离指令中的scope和外部的scope,并且制定外部scope指向指令内部的scope.我们可以通过创建一个独立scope来实现.我们使用指令的scope选项:

>script.js

```javascript
angular.module('docsIsolateScopeDirective', [])
.controller('Controller', ['$scope', function($scope) {
  $scope.naomi = { name: 'Naomi', address: '1600 Amphitheatre' };
  $scope.igor = { name: 'Igor', address: '123 Somewhere' };
}])
.directive('myCustomer', function() {
  return {
    restrict: 'E',
    scope: {
      customerInfo: '=info'
    },
    templateUrl: 'my-customer-iso.html'
  };
});
```

>index.html
```html
<div ng-controller="Controller">
  <my-customer info="naomi"></my-customer>
  <hr>
  <my-customer info="igor"></my-customer>
</div>
```

>my-customer-iso.html

```html
Name: {{customerInfo.name}} Address: {{customerInfo.address}}
```
查看`index.html`,第一个`<my-customer>`元素绑定的`info`属性值是`naomi`,第二个绑定的是`igor`

我们近距离观察下scope选型

```javascript
//...
scope: {
  customerInfo: '=info'
},
//...
```
scope选项是一个对象,它包含一个属性，用于每个隔离范围绑定,在这里他仅仅只有一个属性:
* 他的名字相当于指令的独立作用域属性
* 他的值告诉`$compile`绑定`info`属性

在案例中,如果属性名称和你想要绑定到指令scope的值相同,你可以使用简写:

```
...
scope: {
  // same as '=customer'
  customer: '='
},
...
```

除绑定不同数据到指令的scope之外,还有另一个效果

我们添加另外一个属性`vojta`显示在我们的scope中,并试图在指令模版中使用

>script.js

```javascript
angular.module('docsIsolationExample', [])
.controller('Controller', ['$scope', function($scope) {
  $scope.naomi = { name: 'Naomi', address: '1600 Amphitheatre' };
  $scope.vojta = { name: 'Vojta', address: '3456 Somewhere Else' };
}])
.directive('myCustomer', function() {
  return {
    restrict: 'E',
    scope: {
      customerInfo: '=info'
    },
    templateUrl: 'my-customer-plus-vojta.html'
  };
});
```

>index.html

```html
<div ng-controller="Controller">
  <my-customer info="naomi"></my-customer>
</div>
```

>my-customer-plus-vojta.html

```html
Name: {{customerInfo.name}} Address: {{customerInfo.address}}
<hr>
Name: {{vojta.name}} Address: {{vojta.address}}
```
注意`{{vojta.name}}`和`{{vojta.address}}`为空,以为着他们未定义.虽然我们在控制器中定义了`vojta`,但是在指令中不能获取.

顾名思义,指令独立的scope可以隔离除你明确定义在scope:{}的对象外的一切,当建立重用组件时是很有帮助的,因为它可以阻止再更改模型状态时更改组件,除你明确定义过的模型外

## 创建一个操作dom的指令

在这个例子里我们将创建一个在当前时间展示的指令,每秒更新当前时间映射到dom上.

想要更改dom,通常使用`link`选项注册dom监听以及更新dom.它在模版被克隆后及指令的逻辑插入之处执行.

`link`函数具有以下签名
```function link(scope,element,attrs,controller,transcludeFn){}```
* `scope` 是一个angular的scope对象
* `element`指令所匹配的jglite封装的元素
* `attrs`是一个键值对的hash对象,每一个键值对包含规范化的属性名和相对应的值
* `controller`是指令所需的控制器实例（或其自身控制器）（如果有的话）。确切的值取决于该指令的要求属性。
* `transcludeFn`是一个嵌入的链接函数预绑定到正确的嵌入范围

>想知道更多关于`link`选项,请查看`$compile`API

在我们的`link`函数中,我们想要每秒更新一次时间显示,或者在任何用户改变了我们指令绑定的时间格式字符串.我们使用`$interval`服务周期性的调用handler.这样做比使用`$timeout`简单,并且更好的支持端对端的测试,我们要确保`$timeout`在测试完成之前就要完成,如果指令被删除,我们也要移除`$interval`,所以我们不会引发内存泄漏.

>script.js
```javascript
angular.module('docsTimeDirective', [])
.controller('Controller', ['$scope', function($scope) {
  $scope.format = 'M/d/yy h:mm:ss a';
}])
.directive('myCurrentTime', ['$interval', 'dateFilter', function($interval, dateFilter) {

  function link(scope, element, attrs) {
    var format,
        timeoutId;

    function updateTime() {
      element.text(dateFilter(new Date(), format));
    }

    scope.$watch(attrs.myCurrentTime, function(value) {
      format = value;
      updateTime();
    });

    element.on('$destroy', function() {
      $interval.cancel(timeoutId);
    });

    // start the UI update process; save the timeoutId for canceling
    timeoutId = $interval(function() {
      updateTime(); // update DOM
    }, 1000);
  }

  return {
    link: link
  };
}]);
Load online demo

```

>index.html
```html
<div ng-controller="Controller">
  Date format: <input ng-model="format"> <hr/>
  Current time is: <span my-current-time="format"></span>
</div>
```
这里有两个需要注意的地方,就像module.controller API，在module.directive函数参数的依赖注入。因为这样,我们可以在我们指令的link函数中使用`$interval`和`datefilter`.

我们注册了一个事件
`element.on("$destroy",..)`什么激活这个`$destroy`事件呢

有几个angularjs发出的特别事件,当一个被angular编译器编译的节点被毁掉后,它将发送一个`$destroy`事件.同样,当一个angular scope被销毁时,它会广播一个`$destroy`事件到正在监听的scopes.

通过监听这个事件，您可以删除可能导致内存泄漏的事件侦听器。注册在scope和element当他们在销毁时会自动销毁监听.但是如果你在一个服务上注册了监听,或者在dom节点上注册监听将不会被删除.你需要自己清除或者引起内存泄漏的危险.

## 创建一个封装其它元素的指令

我们已经看到,你可以通过模型进入指令的独立作用域,但有时它是希望能够通过在一个完整的模板，而不是一个字符串或对象。我们想要创建一个"dialog box"组件.这个组件可以封装任意内容.

为了做这个组件,我们需要使用`transclude` 选项

>script.js

```javascript
angular.module('docsTransclusionDirective', [])
.controller('Controller', ['$scope', function($scope) {
  $scope.name = 'Tobias';
}])
.directive('myDialog', function() {
  return {
    restrict: 'E',
    transclude: true,
    scope: {},
    templateUrl: 'my-dialog.html'
  };
});
```
>index.html

```html
<div ng-controller="Controller">
  <my-dialog>Check out the contents, {{name}}!</my-dialog>
</div>
```

>my-dialog.html

```html
<div class="alert" ng-transclude></div>
```
`transclude`选项事实上做了什么呢?
`transclude`使得指令的内容与此选项获得指令外部的scope而不是内部.

举例说明下,查看下面得示例,注意我们添加了`link`函数.它重新定义了`name` 作为`Jeff`.你认为`{{name}}`的绑定会立即解除吗?


>script.js

```javascript
angular.module('docsTransclusionDirective', [])
.controller('Controller', ['$scope', function($scope) {
  $scope.name = 'Tobias';
}])
.directive('myDialog', function() {
  return {
    restrict: 'E',
    transclude: true,
    scope: {},
    templateUrl: 'my-dialog.html',
    link: function(scope){
      scope.name = "Jeff";
    }
  };
});
```
>index.html

```html
<div ng-controller="Controller">
  <my-dialog>Check out the contents, {{name}}!</my-dialog>
</div>
```

>my-dialog.html

```html
<div class="alert" ng-transclude></div>
```

通常,我们会期望那个`{{name}}`是`Jeff`,然而,我们看到这个例子的`{{name}}`依然绑定的是`Tobias`.
>原文:
The transclude option changes the way scopes are nested. It makes it so that the contents of a transcluded directive have whatever scope is outside the directive, rather than whatever scope is on the inside. In doing so, it gives the contents access to the outside scope.
翻译:
`transclude`选项改变scopes的嵌套方式.被嵌入指令的内容有任何scope都在指令的外部.而不是在内部.在这样做时，它提供了内容访问的外部范围

 注意 如果指令没有创建自己的scope,那么`scope`在`scope.name = "Jeff"`将引入外部scope和我们将看到`Jeff`被输出.

这种行为对封装了一些内容的指令是有意义的.因为其它方式,你想进入你想要分开的每一个模块.如果你想进入你要使用的每一个模块,你就不能真正的拥有arbitrary内容,你可以吗?

下来,我们想要添加一个按钮到dialog box 并且允许一些人使用这个标签绑定他们自己的行为

>script.js

```javascript
angular.module('docsIsoFnBindExample', [])
.controller('Controller', ['$scope', '$timeout', function($scope, $timeout) {
  $scope.name = 'Tobias';
  $scope.message = '';
  $scope.hideDialog = function (message) {
    $scope.message = message;
    $scope.dialogIsHidden = true;
    $timeout(function () {
      $scope.message = '';
      $scope.dialogIsHidden = false;
    }, 2000);
  };
}])
.directive('myDialog', function() {
  return {
    restrict: 'E',
    transclude: true,
    scope: {
      'close': '&onClose'
    },
    templateUrl: 'my-dialog-close.html'
  };
});
```
>index.html

```html
<div ng-controller="Controller">
  {{message}}
  <my-dialog ng-hide="dialogIsHidden" on-close="hideDialog(message)">
    Check out the contents, {{name}}!
  </my-dialog>
</div>
```
>my-dialog-close.html
```html
<div class="alert">
  <a href class="close" ng-click="close({message: 'closing for now'})">&times;</a>
  <div ng-transclude></div>
</div>
```
我们想要运行指令作用域引用的函数,但是他只能在注册的scope上下文运行.

我们原来在scope选项中使用过`=attr`,但是我们在上面的例子中使用了`&attr`替代.`&`绑定允许指令触发原scope上下文中定义的表达式,在特定的时间.任何定义的表达式都被允许,包括函数的调用.因此,`&`绑定是一个为指令的行为绑定回调函数的完美方案.

当用户点击x,这个指令`close`函数将被调用,谢谢`ng-click`.他调用了独立作用域的`close`执行表达式的`hideDialog(message)`在定义的scope的上下文里,因此执行了`controller`的`hideDialog`函数.

通常情况下，通过将数据从隔离范围传递到父域，这样做可以通过将局部变量名和值映射到表达式包装器函数中来完成.例如,`hideDialog`函数当对话框隐藏时使信息展示,这是指令中特定的执行`close({message:'closing for now'})`,然后本地变量信息将在`on-close`表达式中可用。
## 创建一个绑定监听的指令

之前,我们使用了`link`函数创建一个操作dom元素的指令,基于上面的例子,我们给指令的元素添加事件

例如,我们想要实现含有用户可拖拽的元素的指令
>script.js

```javascript
angular.module('dragModule', [])
.directive('myDraggable', ['$document', function($document) {
  return {
    link: function(scope, element, attr) {
      var startX = 0, startY = 0, x = 0, y = 0;

      element.css({
       position: 'relative',
       border: '1px solid red',
       backgroundColor: 'lightgrey',
       cursor: 'pointer'
      });

      element.on('mousedown', function(event) {
        // Prevent default dragging of selected content
        event.preventDefault();
        startX = event.pageX - x;
        startY = event.pageY - y;
        $document.on('mousemove', mousemove);
        $document.on('mouseup', mouseup);
      });

      function mousemove(event) {
        y = event.pageY - startY;
        x = event.pageX - startX;
        element.css({
          top: y + 'px',
          left:  x + 'px'
        });
      }

      function mouseup() {
        $document.off('mousemove', mousemove);
        $document.off('mouseup', mouseup);
      }
    }
  };
}]);
Load online demo

```
>index.html
```html
<span my-draggable>Drag Me</span>
```
## 创建一个交互指令

你可以在模版里组合任何指令.

有时,你想要一个创建了指令混合体的组件.

想像你想有一个容器的选项卡，其中的内容的容器对应的那个选项卡是激活的。

>script.js

```javascript
angular.module('docsTabsExample', [])
.directive('myTabs', function() {
  return {
    restrict: 'E',
    transclude: true,
    scope: {},
    controller: ['$scope', function($scope) {
      var panes = $scope.panes = [];

      $scope.select = function(pane) {
        angular.forEach(panes, function(pane) {
          pane.selected = false;
        });
        pane.selected = true;
      };

      this.addPane = function(pane) {
        if (panes.length === 0) {
          $scope.select(pane);
        }
        panes.push(pane);
      };
    }],
    templateUrl: 'my-tabs.html'
  };
})
.directive('myPane', function() {
  return {
    require: '^^myTabs',
    restrict: 'E',
    transclude: true,
    scope: {
      title: '@'
    },
    link: function(scope, element, attrs, tabsCtrl) {
      tabsCtrl.addPane(scope);
    },
    templateUrl: 'my-pane.html'
  };
});
```
>index.html
```html
<my-tabs>
  <my-pane title="Hello">
    <p>Lorem ipsum dolor sit amet</p>
  </my-pane>
  <my-pane title="World">
    <em>Mauris elementum elementum enim at suscipit.</em>
    <p><a href ng-click="i = i + 1">counter: {{i || 0}}</a></p>
  </my-pane>
</my-tabs>
```
>my-tabs.html

```html
<div class="tabbable">
  <ul class="nav nav-tabs">
    <li ng-repeat="pane in panes" ng-class="{active:pane.selected}">
      <a href="" ng-click="select(pane)">{{pane.title}}</a>
    </li>
  </ul>
  <div class="tab-content" ng-transclude></div>
</div>
```
>my-pane.html

```html
<div class="tab-pane" ng-show="selected">
  <h4>{{title}}</h4>
  <div ng-transclude></div>
</div>
```
`myPane`指令有一个`require`选项值为`^^myTabs`.当一个指令使用这个选项,`$compile`将会抛出错误除非指定的Controller被找到.`^^`前缀意思是这个指令寻找他的父元素的controller(`^`前缀表示在指令自己的元素或者父元素查找constroller;没有任何前缀,指令将只查找自己的元素).

`myTabs`controller从哪来呢?指令可以使用被命名的`controller`选项.指定控制器.正如你所看到的,`myTabs`指令使用了这个选项.就像`ngController`,这个选项为指令的模版指定了一个控制器.

如果引用controller或者函数绑定模版的控制器势必要的话.你可以使用controllerAs指定controller的别名.这个指令需要定义个scope被这个配置使用.当指令被当作组件使用的时候这个是特别有用的.

让我回顾以下`myPane`的定义.注意`link`函数的最后一个参数:`tabsCtrl`.当一个指令需要一个controller,他在`link`函数中的第四个参数接收.这有利于`myPane`调用`addPane`函数.

如果操作控制器是被需要的,指令的`require`选项可以接受数组参数.与之对应的参数会以数组的形式传入`link`函数.

```javascript

angular.module('docsTabsExample', [])
.directive('myPane', function() {
  return {
    require: ['^^myTabs', 'ngModel'],
    restrict: 'E',
    transclude: true,
    scope: {
      title: '@'
    },
    link: function(scope, element, attrs, controllers) {
      var tabsCtrl = controllers[0],
          modelCtrl = controllers[1];

      tabsCtrl.addPane(scope);
    },
    templateUrl: 'my-pane.html'
  };
});
```
大部分读者也许好奇`link`和`controller`的区别.基本的区别是`controller`能暴露API,`link`函数使用`require`可以与控制器交互.

>如果你想暴露给其它指令api的话 使用controller,否则使用link