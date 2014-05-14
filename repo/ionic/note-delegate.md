


### Delegate
非常好的实现了控制反转Ioc?(把 ionic 提供的 directive[UI widget等] 的controller方法 给外部应用的controller中调用)
区别注意点是： 通过在 html directive 中通过配置 attr 来声明方法然后其实是通过 callback 的形式进行传递控制

举个例子：

```html
<div ng-controller="contentCtrl">
    <x-panel panel-click="panelFocusHanler()"></x-panel>
</div>
<script type="text/javascript">
    app.controller('contentCtrl', function($scope) {
        $scope.panelFocusHanler = function() {
            // biz logic will be invoked by directive which is by callback style
        };
    });
</script>

```

注意点：

- 对于 controller injectable delegate，被注入 controller 后，都是可以控制全部的相关的 directive view， 所以可以通过 $getByHandle 方法来定位到特定 view（directive）. 而handlename是通过如 `<ion-content delegate-handler="mainScroll"></ion-content>` 来定义
- 对于一些 controller injectable delegate 是通过隐式创建的。
如$ionicScrollDelegate, 可以就可以被 ionContent directive 来隐式创建。

#### 学习关键的Delegate:

##### $ionicScrollDelegate

resize()
Tell the scrollView to recalculate the size of its container.

anchorScroll([shouldAnimate])
Tell the scrollView to scroll to the element with an id matching window.location.hash.

rememberScrollPosition(id)
related: scrollToRememberedPosition, forgetScrollPosition
for pages asscoiated with a ion-nav-item, rememberScrollPosition is automatically enabled.

```js
function ScrollCtrl($scope, $ionicScrollDelegate) {
  var delegate = $ionicScrollDelegate.$getByHandle('myScroll');

  // Put any unique ID here.  The point of this is: every time the controller is recreated
  // we want to load the correct remembered scroll values.
  delegate.rememberScrollPosition('my-scroll-id');
  delegate.scrollToRememberedPosition();
  $scope.items = [];
  for (var i=0; i<100; i++) {
    $scope.items.push(i);
  }
}
```

##### $ionicSideMenuDelegate

首先是 $ionicSideMenus 这个directive 很有趣， 它其是content panel(ion-side-menu-content)和side-menu的wrapper(父亲容器 directive)

getOpenRatio() -> ratio of open amount over menu width
toggleRight()
canDragContent() -> set whether the content can or cannot be dragged to open side menus





