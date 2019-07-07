## ng-options指令的两种用法

1. ```html
   <select ng-model="selectedName" ng-options="item.name for item in items">	
   ```

   * 此时这个表达式的含义是：
     * item.name for ...将name值设置为options下拉中的可见选项
     * item作为$scope.selectedName的值（对象作为数据绑定中的值）

2. ```javascript
   <select ng-model="selectedName" ng-options="item.name as item.name for item in items">
   ```

   * 此时表达式的含义是：
     * item.name as 将item对象的name属性作为selectedName数据绑定的值
     * item.anme for 将name值设置为options下拉中的可见选项
     * <font color="red">当时用该表达式的时候，要注意使用ng-model指令绑定变量，不然由于item.name as 没有赋值的对象就会报错</font>

