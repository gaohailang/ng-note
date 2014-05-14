

### Summary
collection-repeat, is a directive that allows you to render lists with thousands of items in them.


### Tips

collection-repeat 是做性能优化的.
item的尺寸要固定： You must explicitly tell the directive what size your items will be in the DOM, using directive attributes. 
element rendered will be absolutely positioned

provide an optional tracking function which can be used to associate the objects in the collection with the DOM elements. A built in $id() function can be used to assign a unique $$hashKey property to each item in the array. This property is then used as a key to associated DOM elements with the corresponding item in the array by identity. 

```html
<ion-content ng-controller="ContentCtrl">
  <div class="list">
    <div class="item my-item"
      collection-repeat="item in items"
      collection-item-width="'100%'"
      collection-item-height="getItemHeight(item, $index)"
      ng-style="{height: getItemHeight(item, $index)}">
      {{item}}
    </div>
  </div>
</ion-content>

<ion-content>
  <div class="item item-avatar my-image-item"
    collection-repeat="image in images"
    collection-item-width="'33%'"
    collection-item-height="'33%'">
    <img ng-src="">
  </div>
</ion-content>
```

```js
function ContentCtrl($scope) {
  $scope.items = [];
  for (var i = 0; i < 1000; i++) {
    $scope.items.push('Item ' + i);
  }

  $scope.getItemHeight = function(item, index) {
    //Make evenly indexed items be 10px taller, for the sake of example
    return (index % 2) === 0 ? 50 : 60;
  };
}
```