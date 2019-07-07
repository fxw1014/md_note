## $watch

1. 作用：

   * ```javascript
      // 监听一个model 当第一个参数model(name)每次改变时 都会触发第2个参数函数
      // 'name' 等价于 $scope.name
       $scope.$watch('name',function(newValue,oldValue){
           //newValue:参数1改变后的值，oldValue:参数2改变后的值
           //此方法中不能使用 $scope.变量 来做判断
           //当然此处的newValue,oldValue只是个变量名，所以可以改变，但是注意不要使用关键字(new)作为变量名
            ...
         
 }
     ```
     
     

