# Mei
viewport
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no" />
    <title>Title</title>
    <link rel="stylesheet" href="css/reset.css">
    <link rel="stylesheet" href="css/main.css">
    <link rel="stylesheet" href="css/company.css">
    <link rel="stylesheet" href="css/person.css">


</head>
<body ng-app="MyApp" ng-controller="myCtrl">
<ui-view></ui-view>

<script type='text/javascript' src='js/angular.js'></script>
<script type='text/javascript' src='js/angular-ui-router.js'></script>
<script src="app.js"></script>
</body>
</html>
#main css
#main .head{
  height:30px;
  padding: 10px;
  background-color: #12d5b5;
  align-items: center;
  line-height: 30px;
}
#main span{
  color: #ffffff;
  font-size: 20px;
  float:left;
  margin-left: 8px;
}
#main button{
  margin-right: 8px;
  background-color: #f5a200;
  border: none;
  width: 70px;
  height: 30px;
  color: #ffffff;
  float: right;
}
#list li{
  overflow: hidden;
}
#list img{
  float: left;
  width:60px;
  height:60px;
  padding: 5px 8px;
}
#list h4{
  color: #666;
  font-size: 15px;
}
#list .p1{
  padding: 3px;
}
#list .p2{
  color: #f5a200;
}


<div id="main">
  <div class="head">
    <span>10s定制职位</span>
    <button ui-sref="person" >个人中心</button>
  </div>
  <ul id="list">
    <li ng-repeat="item in items" ui-sref="company({id:item.id})">
      <img ng-src='{{item.imgSrc}}' >
      <h4>{{item.name}}</h4>
      <p class="p1">{{item.city}}</p>
      <p class="p2">{{item.time}}</p>
    </li>
  </ul>
</div>

##company css

#company .head{
  background-color: #12d5b5;
  height:30px;
  padding: 5px;
  line-height: 30px;
  text-align: center;
}
#company .sp{
  color: #fff;
  font-size: 25px;
  position: absolute;
  left: 10px;
  width: 30px;
}
#company span{
  color: #fff;
  font-size: 15px;
}

#nav img{
  width:80px;
  height: 80px;
  margin-right: 10px;
  float: left;
}
#nav .middle{
  padding: 3px 0;
}
#nav{
  overflow: hidden;
  padding: 10px;
  border-bottom:1px solid #eee ;
}
#nav p{
  color: #333;
}
#nav .last{
  color:#f5a200 ;
}















<div id="company">
  <div class="head">
    <span class="sp" ng-click="back()">&lt;</span>
    <span>公司信息</span>
  </div>
  <div id="nav">
    <img ng-src="{{item.imgSrc}}" alt="">
    <p>{{item.company}}</p>
    <p class="middle">公司地址: {{item.city}}</p>
    <p class="last">公司类型: {{item.business}}</p>
  </div>
</div>
##person css
#person .head {
  height: 100px;
  background-color: #12d5b5;
  text-align: center;
}
#person .head img{
  width:50px;
  height: 50px;
  border-radius:50% ;
  margin:25px auto
}
#person button{
  border-radius:4px ;
  width:80px;
  height: 30px;
  border: none;
  background-color: #f5a200;
  margin-left:10px;
}
#nav{
  padding: 10px;
}
#nav p{
  color: #333;
  font-size: 15px;
}


<div id="person">
  <div class="head">
    <img src="image/81143108df3a2f82e8110b531083ca51.jpg" alt="">
  </div>
  <div id="nav">
    <p>姓名 ：<span>库里</span></p>
    <p class="middle">年龄 ：<span>29</span></p>
    <p>邮箱 ：<span>curry@163.com</span></p>
  </div>
  <button ui-sref="main" >回到主页</button>
</div>





/**
 * Created by Administrator on 2017/4/19.
 */
 
 
 
  angular.module('MyApp',['ui.router'])
      .config(function ($stateProvider,$urlRouterProvider) {
        $stateProvider
            //路由id
            .state('main',{
              url : '/main',
              templateUrl : './template/main.html',
              controller : 'MainCtrl'
            })
            .state('company',{
              url : '/company/:id',
              templateUrl : './template/company.html',
              controller : 'CompanyCtrl'
            })
            .state('person',{
              url:'/person',
              templateUrl:'./template/person.html',
              controller:'PersonCtrl'
            })

        $urlRouterProvider.otherwise('/main');


      })
    .controller('MainCtrl',function ($scope,$http) {
      //初始化公司信息
      $http({
          method:'get',
          url:'http://localhost:3000/items',
          })
          .success(function (items) {
            $scope.items = items;
          })
          .error(function (error) {
            console.log(error)
          })
    })
      .controller('CompanyCtrl',function ($scope,$http,$stateParams) {
        var index = $stateParams.id-1;
        console.log($stateParams);
        $http({
          method:'get',
          url:'http://localhost:3000/items',
        })
            .success(function (items) {
              $scope.item = items[index];
            })
            .error(function (error) {
              console.log(error)
            });
        //点击回退事件
        $scope.back = function () {
          window.history.back();
        }
      })
      .controller('PersonCtrl',function ($scope) {

      })
    .controller('myCtrl',function ($scope) {

    })
