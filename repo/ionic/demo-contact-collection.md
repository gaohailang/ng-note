


```html
<body ng-controller="MainCtrl">
  <ion-header-bar class="bar-positive">
    <h1 class="title">3000 Contacts</h1>
    <div class="button" ng-click="scrollBottom()">
      Scroll To Bottom
    </div>
  </ion-header-bar>
  <ion-header-bar class="bar-light bar-subheader">
    <input type="search"
      placeholder="Filter contacts..."
      ng-model="search"
      ng-focus="searchFocused = true"
      ng-blur="searchFocused = false"
      ng-change="scrollTop()">
    <button ng-if="search.length"
      class="button button-icon ion-android-close input-button"
      ng-click="clearSearch()">
    </button>
  </ion-header-bar>
  <ion-content>
    <div class="list">
      <a class="item my-item"
        collection-repeat="item in getContacts()"
        collection-item-height="getItemHeight(item)"
        collection-item-width="getItemWidth(item)"
        ng-href="https://www.google.com/#q={{item.first_name + '+' + item.last_name}}"
        ng-style="{'line-height': getItemHeight(item) + 'px'}"
        ng-class="{'item-divider': item.isLetter}">
        <img ng-if="!item.isLetter" ng-src="http://placekitten.com/60/{{55 + ($index % 10)}}">
        {{item.letter || (item.first_name+' '+item.last_name)}}
      </a>
    </div>
  </ion-content>
</body>
```

```js
angular.module('contactsApp', ['ionic'])
.controller('MainCtrl', function($scope, $ionicScrollDelegate, filterFilter) {
  var letters = $scope.letters = [];
  var contacts = $scope.contacts = [];
  var currentCharCode = 'A'.charCodeAt(0) - 1;

  //window.CONTACTS is defined below
  window.CONTACTS
    .sort(function(a, b) {
      return a.last_name > b.last_name ? 1 : -1;
    })
    .forEach(function(person) {
      //Get the first letter of the last name, and if the last name changes
      //put the letter in the array
      var personCharCode = person.last_name.toUpperCase().charCodeAt(0);
      //We may jump two letters, be sure to put both in
      //(eg if we jump from Adam Bradley to Bob Doe, add both C and D)
      var difference = personCharCode - currentCharCode;
      for (var i = 1; i <= difference; i++) {
        addLetter(currentCharCode + i);
      }
      currentCharCode = personCharCode;
      contacts.push(person);
    });

  //If names ended before Z, add everything up to Z
  for (var i = currentCharCode + 1; i <= 'Z'.charCodeAt(0); i++) {
    addLetter(i);
  }

  function addLetter(code) {
    var letter = String.fromCharCode(code);
    contacts.push({
      isLetter: true,
      letter: letter
    });
    letters.push(letter);
  }

  //Letters are shorter, everything else is 52 pixels
  $scope.getItemHeight = function(item) {
    return item.isLetter ? 40 : 100;
  };
  $scope.getItemWidth = function(item) {
    return '100%';
  };

  $scope.scrollBottom = function() {
    $ionicScrollDelegate.scrollBottom(true);
  };

  var letterHasMatch = {};
  $scope.getContacts = function() {
    letterHasMatch = {};
    //Filter contacts by $scope.search.
    //Additionally, filter letters so that they only show if there
    //is one or more matching contact
    return contacts.filter(function(item) {
      var itemDoesMatch = !$scope.search || item.isLetter ||
        item.first_name.toLowerCase().indexOf($scope.search.toLowerCase()) > -1 ||
        item.last_name.toLowerCase().indexOf($scope.search.toLowerCase()) > -1;

      //Mark this person's last name letter as 'has a match'
      if (!item.isLetter && itemDoesMatch) {
        var letter = item.last_name.charAt(0).toUpperCase();
        letterHasMatch[letter] = true;
      }

      return itemDoesMatch;
    }).filter(function(item) {
      //Finally, re-filter all of the letters and take out ones that don't
      //have a match
      if (item.isLetter && !letterHasMatch[item.letter]) {
        return false;
      }
      return true;
    });
  };

  $scope.clearSearch = function() {
    $scope.search = '';
  };
});
```