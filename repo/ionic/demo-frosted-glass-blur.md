

### Summary
frosted glasses effect was made popular in iOS 7. - bulurring not only the top layer, but the content underneath it.

All you need to do is include the CSS and JS from the Ionic Frosted Glass repo, include the ionic.contrib.frostedGlass angular module, and then add frosted-bar to your header bar in Ionic


```html
<body>
  <ion-pane ng-controller="PageCtrl">
    <header frosted-bar class="bar bar-header bar-frosted">
      <h1 class="title">Blurred!</h1>
    </header>
    <ion-content padding="true" class="has-header" start-y="120">
      <ol class="messages">
        <li ng-repeat="message in messages" ng-bind-html="message.content"></li>
      </ol>
    </ion-content>
    <ion-footer-bar class="bar-frosted">
      <button class="button button-clear button-positive" ng-click="add()">Add Message</button>
    </ion-footer-bar>
  </ion-pane>
</body>
```

```js
angular.module('starter', ['ionic', 'ionic.contrib.frostedGlass'])

.controller('PageCtrl', function($scope, $ionicFrostedDelegate, $ionicScrollDelegate, $rootScope) {
  var messageOptions = [
    { content: '<p>Wow, this is really something huh?</p>' },
    { content: '<p>Yea, it\'s pretty sweet</p>' },
    { content: '<p>I think I like Ionic more than I like ice cream!</p>' },
    { content: '<p>Gee wiz, this is something special.</p>' },
    { content: '<img src="" alt=""/>' },
    { content: '<p>Is this magic?</p>' },
    { content: '<p>Am I dreaming?</p>' },
    { content: '<img src="" alt=""/>'},
    { content: '<p>Am I dreaming?</p>' },
    { content: '<p>Yea, it\'s pretty sweet</p>' },
    { content: '<p>I think I like Ionic more than I like ice cream!</p>' },
  ];
 
  var messageIter = 0;
  $scope.messages = messageOptions.slice(0, messageOptions.length); // cool, so we clone it righ!

  $scope.add = function() {
    var nextMessage = messageOptions[messageIter++ % messageOptions.length];
    $scope.messages.push(angular.extend({}, nextMessage));  // cool, by value or by reference

    // Update the scroll area and tell the frosted glass to redraw itself
    $ionicFrostedDelegate.update();
    $ionicScrollDelegate.scrollBottom(true); // cool, intro by ion-panel, scrollBottom(true)
  };
});
```


```css
body {
  cursor: url('http://ionicframework.com/img/finger.png'), auto;
}

.messages {
  margin:0;
  padding:0;
  list-style-type:none;
}

.messages li {
  display:block;
  float:left;
  clear:both;
  max-width:65%;
  margin:0 0 1rem 0;
  padding:0;
}

.messages li:nth-child(even) {
  float:right;
}

.messages li:nth-child(even) img {
  float:right;
}

.messages p {
  border-radius:.75rem;
  background:#e6e5eb;
  color:#383641;
  padding:.6875rem;
  margin:0;
  font-size:.875rem;
}

.messages li:nth-child(even) p {
  background:#158ffe;
  color:#fff;
}

.messages img {
  display:block;
  max-width:65%;
  border-radius:.75rem;
}
```