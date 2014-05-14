
http://codepen.io/ionic/pen/AjgEB


```js
angular.module('ionicApp', ['ionic'])

.config(function($stateProvider, $urlRouterProvider) {

  $stateProvider
  .state('intro', {
    url: '/',
    templateUrl: 'intro.html',
    controller: 'IntroCtrl'
  })
  .state('main', {
    url: '/main',
    templateUrl: 'main.html',
    controller: 'MainCtrl'
  });

  $urlRouterProvider.otherwise("/");

})

.controller('IntroCtrl', function($scope, $state, $ionicSlideBoxDelegate) {
 
  // Called to navigate to the main app
  $scope.startApp = function() {
    $state.go('main');
  };
  $scope.next = function() {
    $ionicSlideBoxDelegate.next();
  };
  $scope.previous = function() {
    $ionicSlideBoxDelegate.previous();
  };

  // Called each time the slide changes
  $scope.slideChanged = function(index) {
    $scope.slideIndex = index;
  };
})

.controller('MainCtrl', function($scope, $state) {
  console.log('MainCtrl');
  
  $scope.toIntro = function(){
    $state.go('intro');
  }
});

```


```html
<body ng-app="ionicApp" animation="slide-left-right-ios7">

  <ion-nav-bar class="nav-title-slide-ios7 bar-light">
      <ion-nav-back-button class="button-icon ion-arrow-left-c">
      </ion-nav-back-button>
  </ion-nav-bar>

  <ion-nav-view></ion-nav-view>

  <script id="intro.html" type="text/ng-template"> 
    <ion-view>
      <ion-nav-buttons side="left">
        <button class="button button-positive button-clear no-animation"
                ng-click="startApp()" ng-if="!slideIndex">
          Skip Intro
        </button>
        <button class="button button-positive button-clear no-animation"
                ng-click="previous()" ng-if="slideIndex > 0">
          Previous Slide
        </button>
      </ion-nav-buttons>
      <ion-nav-buttons side="right"> 
        <button class="button button-positive button-clear no-animation"
                ng-click="next()" ng-if="slideIndex != 2">
          Next
        </button>
        <button class="button button-positive button-clear no-animation"
                ng-click="startApp()" ng-if="slideIndex == 2">
          Start using MyApp
        </button>
      </ion-nav-buttons>
      <ion-slide-box on-slide-changed="slideChanged(index)">
        <ion-slide>
          <h3>Thank you for choosing the Awesome App!</h3>
          <div id="logo">
            <img src="http://code.ionicframework.com/assets/img/app_icon.png">
          </div>
          <p>
            We've worked super hard to make you happy.
          </p>
          <p>
            But if you are angry, too bad.
          </p>
        </ion-slide>
        <ion-slide>
          <h3>Using Awesome</h3>
          
          <div id="list">
            <h5>Just three steps:</h5>
            <ol>
              <li>Be awesome</li>
              <li>Stay awesome</li>
              <li>There is no step 3</li>
            </ol>
          </div>
        </ion-slide>
        <ion-slide>
          <h3>Any questions?</h3>
          <p>
            Too bad!
          </p>
        </ion-slide>
      </ion-slide-box>
    </ion-view>
  </script>

  <script id="main.html" type="text/ng-template">
    <ion-view hide-back-button="true" title="Awesome">
      <ion-content padding="true">
        <h1>Main app here</h1>
        <button class="button" ng-click="toIntro()">Do Tutorial Again</button>
      </ion-content>
    </ion-view>
  </script>
  
</body>
```


```css
body {
  cursor: url('http://ionicframework.com/img/finger.png'), auto;
}
.slider {
  height: 100%;
}
.slider-slide {
  padding-top: 80px;
  color: #000;
  background-color: #fff;
  text-align: center;

  font-family: "HelveticaNeue-Light", "Helvetica Neue Light", "Helvetica Neue", Helvetica, Arial, "Lucida Grande", sans-serif; 
  font-weight: 300;
}

#logo {
  margin: 30px 0px;
}

#list {
  width: 170px;
  margin: 30px auto;
  font-size: 20px;
}
#list ol {
  margin-top: 30px;
}
#list ol li {
  text-align: left;
  list-style: decimal;
  margin: 10px 0px;
}
```