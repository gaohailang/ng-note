
```html
<body ng-controller="FlickrCtrl">

  <ion-header-bar class="bar-dark"><h1 class="title">Flickr Search</h1>
  </ion-header-bar>

  <div id="search-bar">
    <div class="item item-input-inset">
      <label class="item-input-wrapper" id="search-input">
        <i class="icon ion-search placeholder-icon"></i>
        <input type="text" placeholder="Search" ng-model="query" ng-change="search()">
      </label>
    </div>
  </div>

  <ion-content id="content" push-search>
    <div id="photos" class="clearfix">
      <div class="photo" ng-repeat="photo in photos.items">
        <img ng-src="{{ photo.media.m }}">
      </div>
    </div>
  </ion-content>

</body>
```


```js
angular.module('ionicApp', ['ionic', 'ngResource'])

.factory('Flickr', function($resource, $q) {
  var photosPublic = $resource('http://api.flickr.com/services/feeds/photos_public.gne', 
      { format: 'json', jsoncallback: 'JSON_CALLBACK' }, 
      { 'load': { 'method': 'JSONP' } });
      
  return {
    search: function(query) {
      var q = $q.defer();
      photosPublic.load({
        tags: query
      }, function(resp) {
        q.resolve(resp);
      }, function(err) {
        q.reject(err);
      })
      
      return q.promise;
    }
  }
})

.controller('FlickrCtrl', function($scope, Flickr) {

  // ionic 里面居然集成了 debounce 方法....
  var doSearch = ionic.debounce(function(query) {
    Flickr.search(query).then(function(resp) {
      $scope.photos = resp;
    });
  }, 500);
  
  $scope.search = function() {
    doSearch($scope.query);
  };

})

.directive('pushSearch', function() {
  // 把 search-bar 推走，当开始滚动到下方的时候
  return {
    restrict: 'A',
    link: function($scope, $element, $attr) {
      var amt, st, header;

      $element.bind('scroll', function(e) {
        if(!header) {
          header = document.getElementById('search-bar');
        }
        st = e.detail.scrollTop;
        if(st < 0) {
          header.style.webkitTransform = 'translate3d(0, 0px, 0)';
        } else {
          header.style.webkitTransform = 'translate3d(0, ' + -st + 'px, 0)';
        }
      });
    }
  }
})

.directive('photo', function($window) {
  return {
    restrict: 'C',
    link: function($scope, $element, $attr) {
      var size = ($window.outerWidth / 3) - 2;
      $element.css('width', size + 'px');
    }
  }
});
```
