

引入了外部库支持：
http://code.ionicframework.com/contrib/ionic-contrib-swipecards/ionic.swipecards.js


```html
<body ng-app="starter" class="slide-left-right-ios7" no-scroll>

  <ion-pane ng-controller="CardsCtrl">
    <ion-header-bar class="bar-transparent">
      <h1 class="title">Help Out</h1>
    </ion-header-bar>

    <swipe-cards>
      <swipe-card on-swipe="cardSwiped()" id="start-card">
        Swipe down for a new card
      </swipe-card>
      <swipe-card ng-repeat="card in cards" on-destroy="cardDestroyed($index)" on-swipe="cardSwiped($index)">

        <div ng-controller="CardCtrl">
          <div class="title">
            {{card.title}}
          </div>
          <div class="image">
            <img ng-src="{{card.image}}">
          </div>
          <div class="button-bar">
            <button class="button button-clear button-positive" ng-click="goAway()">Answer</button>
            <button class="button button-clear button-positive" ng-click="goAway()">Decline</button>
          </div>
        </div>
      </swipe-card>
    </swipe-cards>
  </ion-pane>

  <!-- quick cache hack -->
  <img src="http://ionicframework.com.s3.amazonaws.com/demos/ionic-contrib-swipecards/pic.png" style="display: none">
  <img src="http://ionicframework.com.s3.amazonaws.com/demos/ionic-contrib-swipecards/pic1.png" style="display: none">
  <img src="http://ionicframework.com.s3.amazonaws.com/demos/ionic-contrib-swipecards/pic2.png" style="display: none">
  <img src="http://ionicframework.com.s3.amazonaws.com/demos/ionic-contrib-swipecards/pic3.png" style="display: none">
  <img src="http://ionicframework.com.s3.amazonaws.com/demos/ionic-contrib-swipecards/pic4.png" style="display: none">
</body>
```

```js
angular.module('starter', ['ionic', 'ionic.contrib.ui.cards'])

.directive('noScroll', function($document) {

  return {
    restrict: 'A',
    link: function($scope, $element, $attr) {

      $document.on('touchmove', function(e) {
        e.preventDefault();
      });
    }
  }
})

.controller('CardsCtrl', function($scope, $ionicSwipeCardDelegate) {
  var cardTypes = [
    { title: 'Swipe down to clear the card', image: 'http://ionicframework.com.s3.amazonaws.com/demos/ionic-contrib-swipecards/pic.png' },
    { title: 'Where is this?', image: 'http://ionicframework.com.s3.amazonaws.com/demos/ionic-contrib-swipecards/pic.png' },
    { title: 'What kind of grass is this?', image: 'http://ionicframework.com.s3.amazonaws.com/demos/ionic-contrib-swipecards/pic2.png' },
    { title: 'What beach is this?', image: 'http://ionicframework.com.s3.amazonaws.com/demos/ionic-contrib-swipecards/pic3.png' },
    { title: 'What kind of clouds are these?', image: 'http://ionicframework.com.s3.amazonaws.com/demos/ionic-contrib-swipecards/pic4.png' }
  ];

  $scope.cards = Array.prototype.slice.call(cardTypes, 0, 0);

  $scope.cardSwiped = function(index) {
    $scope.addCard();
  };

  $scope.cardDestroyed = function(index) {
    $scope.cards.splice(index, 1);
  };

  $scope.addCard = function() {
    var newCard = cardTypes[Math.floor(Math.random() * cardTypes.length)];
    newCard.id = Math.random();
    $scope.cards.push(angular.extend({}, newCard));
  }
})

.controller('CardCtrl', function($scope, $ionicSwipeCardDelegate) {
  $scope.goAway = function() {
    var card = $ionicSwipeCardDelegate.getSwipebleCard($scope);
    card.swipe();
  };
});


```