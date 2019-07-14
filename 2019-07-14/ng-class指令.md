## ng-class属性

作用：控制标签属性（增加或删除）

使用方式：<li ng-class="{disabled:isA}" ></li>

* 当isA是true时，这个li状态将会是不可用的。
  * isA可以是数据绑定的值进行的判断，或者方法。
* ==注意ng-class="{}"，双引号中必须有{}，不然会报解析异常错误。==

参考：[https://blog.csdn.net/jumtre/article/details/50802136]()

