## $location服务，页面跳转带参数要在?前加#

```javascript
url：http:``//127.0.0.1:7001/liuxu/pages/main.html?name=5 
1.获取绝对路径 
$location.absUrl();  
//url：http://127.0.0.1:7001/liuxu/pages/main.html?name=5

2.获取主机 
$location.host(); 
http:``//127.0.0.1

3.获取端口号 
$location.port(); 
//7001 

4.获取文本传输协议 
$location.protocol(); 
//http

5. 获取url参数 
$location.search().name或者$location.search()['name'] 
//5 

6.获取url 
$location.url() 
//:/liuxu/pages/main.html?name=5
```

![1562567724378](assets/1562567724378.png)

==需要在接收参数的页面(controller)中引入$location服务。==