

### Summary
该部分包括：

- ion-nav-view
- ion-view
- ion-nav-bar
- ion-nav-back-button
- nav-clear
- $ionicNavBarDelegate

它们之间的关系是：
ion-nav-view
    ion-view
        ion-content
ion-nav-bar
ion-nav-back-button
nav-clear

Ionic use the AngularUI router module so app interfaces can be organized into various "states". 
in state manager, states are bound to named, nested, and parallel views, allowing more than one template to rendered on the same page.(according to url router).
each state is not required to be bound to a URL, and data can be pushed to each state which allows much flexibility. `非常赞`
good to do because the template will be cached for very fast loading, instead of having to fetch them from the network.


ionNavBar directive， 会根据navigation的状态自动更新（如，显示返回按钮之类的）


可以在ion-nav-view 上添加如 animation='slide-left-right' 来控制页面 transition的动画


#### 扩展阅读 - Angular UI-Router

UI router's backstory
current option, ngRoute module
compare ui.router to ngRoute
nested states
state activation
how to activate a state
state urls
multiple named views
abstract states
populartiy
challenges
resources

$state, $stateProvider, $urlRouterProvider

$stateProivder.state('name', {
    url: '',
    template: '',
    resolve: fn,
    controller: fn
});

相同点：
associate a url(optional in ui-router)
apply a template/templateUrl
can assign controller
can resolve dependencies before controller instantates
redirect with when()
handle invalid urls with otherwise()
url parameters



