
### Summary
list 包括如下部分：

- ion-list
- ion-item

- ion-delte-button
- ion-reorder-button
- ion-option-button

- colleciton-repeat
- $ionicListDelegate


### Documentation

can-swipe -> 设定是否可以swipe to reveal option buttons
show-reorder, show-deelte -> 绑定来控制显示与否

```html
<body ng-controller="MyCtrl">
  <ion-header-bar class="bar-positive">
    <div class="buttons">
      <button class="button button-icon icon ion-ios7-minus-outline"
        ng-click="data.showDelete = !data.showDelete; data.showReorder = false"></button>
    </div>
    <h1 class="title">Ionic Delete/Option Buttons</h1>
    <button class="button"
        ng-click="data.showDelete = false; data.showReorder = !data.showReorder">
          Reorder
    </button>
  </ion-header-bar>

  <ion-content>
    <ion-list show-delete="data.showDelete" show-reorder="data.showReorder">
      <ion-item ng-repeat="item in items" item="item" href="#/item/{{item.id}}">
        Item {{ item.id }}
        <ion-delete-button class="ion-minus-circled" ng-click="onItemDelete(item)"></ion-delete-button>
        <ion-option-button class="button-assertive" ng-click="edit(item)">Edit</ion-option-button>
        <ion-option-button class="button-calm" ng-click="share(item)"> Share </ion-option-button>
        <ion-reorder-button class="ion-navicon" on-reoder="moveItem(item, fromIndex, toIndex)"></ion-reorder-button>
      </ion-item>
    </ion-list>
  </ion-content>
</body>
```


```
angular.module('ionicApp', ['ionic'])

.controller('MyCtrl', function($scope) {

  $scope.data = {
    showDelete: false
  };

  $scope.edit = function(item) {
    alert('Edit Item: ' + item.id);
  };
  $scope.share = function(item) {
    alert('Share Item: ' + item.id);
  };

  $scope.moveItem = function(item, fromIndex, toIndex) {
    $scope.items.splice(fromIndex, 1);
    $scope.items.splice(toIndex, 0, item);
  };

  $scope.onItemDelete = function(item) {
    $scope.items.splice($scope.items.indexOf(item), 1);
  };
});
```