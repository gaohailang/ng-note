
### Summary
power a multi-tabbed interface with a Tab Bar and a set of pages that can be tabbed through.

#### ion-tab

- title, href
- icon, icon-on, icon-off
- badge, badge-style,
- on-select, on-deselect, ng-click

可以通过ion-tab 包含住 tab 内容， 也可以通过href属性来指定目标link的内容内嵌为 tab content



```html
<body ng-controller="RootCtrl">
  
  <ion-tabs class="tabs-icon-only tabs-positive">

    <ion-tab title="Home" 
             icon-on="ion-ios7-filing" 
             icon-off="ion-ios7-filing-outline" 
             ng-controller="HomeCtrl">
      
      <ion-header-bar class="bar-positive">
        <button class="button button-clear" ng-click="newTask()">New</button>
        <h1 class="title">Tasks</h1>
      </ion-header-bar>
      
      <ion-content has-tabs="true" on-refresh="onRefresh()">
        <ion-refresher></ion-refresher>
        <ion-list scroll="false" on-refresh="onRefresh()"
                  s-editing="isEditingItems" 
                  animation="fade-out"
                  delete-icon="icon ion-minus-circled">
          <ion-item ng-repeat="item in items"
                    item="item"
                    buttons="item.buttons"
                    can-delete="true"
                    can-swipe="true"
                    on-delete="deleteItem(item)"
                    ng-class="{completed: item.isCompleted}">
            {{item.title}}
            <i class="{{item.icon}}"></i>
          </ion-item>
        </ion-list>
      </ion-content>
    </ion-tab>

    <ion-tab title="About" icon-on="icon ion-ios7-clock" icon-off="icon ion-ios7-clock-outline">
      <header class="bar bar-header bar-positive">
        <h1 class="title">Deadlines</h1>
      </header>
      <ion-content has-header="true">
        <h1>Deadlines</h1>
      </ion-content>
    </ion-tab>

    <ion-tab title="Settings" icon-on="icon ion-ios7-gear" icon-off="icon ion-ios7-gear-outline">
        <!-- small the above -->
    </ion-tab>
    
  </ion-tabs>

  <script id="newTask.html" type="text/ng-template">
    <div id="task-view" class="modal slide-in-up" ng-controller="TaskCtrl">
      <header class="bar bar-header bar-secondary">
        <h1 class="title">New Task</h1>
        <button class="button button-clear button-primary" ng-click="close()">Done</button>
      </header>
      <ion-content class="padding has-header">
        <input type="text" placeholder="I need to do this...">
      </ion-content>
    </div>
  </script>

</body>
```

```js
angular.module('ionicApp', ['ionic'])

.controller('RootCtrl', function($scope) {
  $scope.onControllerChanged = function(oldController, oldIndex, newController, newIndex) {
    console.log('Controller changed', oldController, oldIndex, newController, newIndex);
    console.log(arguments);
  };
})


.controller('HomeCtrl', function($scope, $timeout, $ionicModal, $ionicActionSheet) {
  $scope.items = [];

  $ionicModal.fromTemplateUrl('newTask.html', function(modal) {
    $scope.settingsModal = modal;
  });

  $scope.onReorder = function(el, start, end) {
    ionic.Utils.arrayMove($scope.items, start, end);
  };

  $scope.onRefresh = function() {
    console.log('ON REFRESH');

    $timeout(function() {
      $scope.$broadcast('scroll.refreshComplete');
    }, 1000);
  }

  $scope.newTask = function() {
    $scope.settingsModal.show();
  };

  // Create the items
  for(var i = 0; i < 25; i++) {
    $scope.items.push({
      title: 'Task ' + (i + 1),
      buttons: [{
        text: 'Done',
        type: 'button-success',
        onButtonClicked: completeItem,
      }, {
        text: 'Delete',
        type: 'button-danger',
        onButtonClicked: removeItem,
      }]
    });
  }

})

.controller('TaskCtrl', function($scope) {
  $scope.close = function() {
    $scope.modal.hide();
  }
});

```
