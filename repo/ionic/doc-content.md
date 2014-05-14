

### Summary
包含如下部分：

- ion-content
- ion-refresher: allow you to add pull-to-refresher to a scrollView(place it as the first child of ionContent, ionScroll element)
- ion-pane


API:

- on-refresh, on-pulling: the first is called when the user pulls down enough and lets go of the refresher, the latter is called when the user starts to pull down on the refresher
- pulling-icon, pulling-text
- refreshing-icon, refreshing-text

```html
<ion-content ng-controller="MyController">
  <ion-refresher pulling-text="Pull to refresh..."
    on-refresh="doRefresh()">
  </ion-refresher>
  <ion-list>
    <ion-item ng-repeat="item in items"></ion-item>
  </ion-list>
</ion-content>
```

```js
angular.module('testApp', ['ionic'])
.controller('MyController', function($scope, $http) {
  $scope.items = [1,2,3];
  $scope.doRefresh = function() {
    $http.get('/new-items')
     .success(function(newItems) {
       $scope.items = newItems;
     })
     .finally(function() {
       // Stop the ion-refresher from spinning
       $scope.$broadcast('scroll.refreshComplete');
     });
  };
});
```