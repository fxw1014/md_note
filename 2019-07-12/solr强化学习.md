# Solr-强化学习

## 1. 实现品优购搜索结果高亮显示功能

### 1.1 需求分析

* 将查询结果对应的查询条件进行高亮显示。

### 1.2 后端实现

* 思路：将高亮后端字段赋值给docs数组中对应的字段

* 高亮提供的各个api，queryForHighlightPage(...),SimpleHightlightQuery(...),HighlightOptions...

  * ==queryForHighlihtPage(...)==该方法返回两组相关的数据==docs数组对象,highlighting对象==

    ```javascript
//通过solr提供的web页面，高亮查询的结果，查询字段和值--->item_brand:华为
    "response": {
      "numFound": 66,
      "start": 0,
        "docs": [
      {
            "id": "919669",
          "item_title": "华为 Ascend P6 (P6-T00) 黑色 移动3G手机",
            "item_price": 1289,
            "item_image": "http://img12.360buyimg.com/n1/s450x450_jfs/t3034/299/2060854617/119711/577e85cb/57d11b6cN1fd1194d.jpg",
            "item_goodsid": 1,
            "item_category": "手机",
            "item_brand": "华为",
            "item_seller": "华为",
            "_version_": 1638719682407039000
          },
          {
            "id": "1082721",
            "item_title": "华为 麦芒B199 深灰色 电信3G手机 双模双待双通",
            "item_price": 1269,
            "item_image": "http://img12.360buyimg.com/n1/s450x450_jfs/t3034/299/2060854617/119711/577e85cb/57d11b6cN1fd1194d.jpg",
            "item_goodsid": 1,
            "item_category": "手机",
            "item_brand": "华为",
            "item_seller": "华为",
            "_version_": 1638719682500362200
          }
        ]
      },
      "highlighting": [
    
       "917460": [
           "item_title": [
            "<em>华为</em> P6 (P6-C00) 黑 电信3G手机 双卡双待双通"
            ],
    	  ...
        ],
    
    	"917770": [
          "item_title": [
           "<em>华为</em> P6-C00 电信3G手机（粉色） CDMA2000/GSM 双模双待双通"
          ]
        ]
    
      ]
    
    	
    ```
    
    
    
  * 设置：高亮选项对象 HighlightOptions
  
    1. 创建高亮选项对象，并设置给哪个字段设置高亮==可以设置多个条件==
  
    * 设置高亮标签前缀
    * 设置高亮标签后缀
  
  * 获取高亮入口集合(每条记录的高亮入口)List<HighlightEntry<>TbItem>，得到高亮数据
  
    1. 遍历高亮入口集合，获取高亮列表(高亮的个数)List<Highlight>
2. 遍历高亮类表，获得高亮数据集合List<String>

### 1.3 前端实现

* <font color="red">但是前段的数据展示无法得到高亮的样式，而是将标签作为字符显示到前段，解决方案：</font>

  1. ==$sce==:angularJs的信任策略 ，通过trustAsHtml方法可以将文本JS转换为可以==运行的JS==。

  2. 将文本JS转换为可以运行的JS的功能具有通用性，所以此处使用==angularJs的过滤器==来简化开发,将其定义到base.js中
     * 过滤器的定义中加入了$sce，其中过滤器相当于全局方法。
  3. ng-bind-html指令：用于显示html内容。
  4. 调用过滤器，类似于linux命令中的管道功能，==使用|==

> 补充：ng-bind-html指令，参考：[https://www.imooc.com/article/27271]()
>
> 在为html标签绑定数据的时，如果绑定的内容是纯文本，你可以使用{{}}或者ng-bind。==但在为html标签绑定带html标签的内容的时候==，angularjs为了安全考虑，不会将其渲染成html，而是将其当做文本直接在页面上展示,<font color="red">**ng-bind-html** 指令是通一个安全的方式将内容绑定到 HTML 元素上，在直接使用ng-bind-html指令直接绑定带html标签的数据时候，如果不做安全检测，则会报错，解决方案有如下两种</font>
>
>  1. 引入ngSanitize模块
>
> ```javascript
> //引入外部模块
> <script src="http://apps.bdimg.com/libs/angular.js/1.5.0-beta.0/angular-sanitize.min.js"></script>
> //使用 
> <script>
>     angular.module("myapp", ["ngSanitize"]).controller("MyController", function ($scope) {
>     $scope.content = "<h1>Hello world.</h1>";
>     $scope.txt = "Hello txt world";
>     });
> </script>
> 
> ```
>
>    2. 使用$sce服务和自定义过滤器
>
> ```javascript
> 使用方式同前端实现。
> ```
>
> 

### 1.4 代码改进

* 抽取方法

## 2. 商品分类列表

### 2.1 分析

* 使用类似于==mysql中的分组查询(group by)==。

### 2.2 后端实现

* 有点类似于高亮的后端实现

### 2.3 前端实现

* 不需要修改js代码，只需要在页面上给值==resultMap==
  * resultMap.categoryList

## 3. 品牌和规格列表

### 3.1 缓存品牌和规格列表

#### 3.1.1 分析

- 根据分类表(tbitem_cat)缓存模板ID
- 根据模板ID缓存品牌列表
- 根据模板ID缓存规格列表

#### 3.1.2 后端实现

* 操作pinyougou-sellergoods-service中的itemCatServiceImpl.java中的findByparentId(...)方法，
  * 因为==每次执行完毕增删改操作后都需要重新加载页面，所以都要调用这个方法==,达到将==商品分类存入缓存==，使用==hash结构存储==，==键：分类名，值：模板ID==
* 修改 pinyougou-sellergoods-service 的 TypeTemplateServiceImpl.java中的findPage(...)方法
  * 根据模板ID缓存品牌列表，==键：模板ID，值：品牌列表==
  * 根据模板ID缓存规格列表，==键：模板ID，值：规格列表==
* 加载缓存数据
  * 在上述缓存方法中向控制台打印对应的消息，查看控制台判断是否缓存成功。

### 3.2 显示品牌和规格列表

#### 3.2.1 分析

* 当在搜索栏输入关键字搜索，==品牌列表和规格列表默认是根据第一个商品分类变化的(不选择商品分类)==，所以在从redis数据库中查询的时候可以根据==第一个商品分类查询品牌列表和规格列表==

#### 3.2.2 后端实现

```java
//查询品牌和规格列表
if(categoryList.size()>0){
    //注意此处的get(0),便是第一次加载的时候，根据第一个商品分类查询品牌列表和规格列表
    map.putAll(searchBrandAndSpecList(categoryList.get(0)));
}
```



#### 3.2.3 前端实现

* 仅修改search.html页面即可。
  * 填充品牌和规格相关数据

## 4. 过滤查询条件构建(前端)

### 4.1 分析

* 点击搜索面板上的分类、品牌和规格，实现查询条件的构建。查询条件以面包屑的形式显示。
* 当面包屑显示分类、品牌和规格时，要同时隐藏搜索面板对应的区域。
* 用户可以点击面包屑上的关闭(x) 撤销查询条件。撤销后显示搜索面包相应的区域。

### 4.2 实现

* 构造搜索对象，resultMap

## 5. 过滤查询(后端)









