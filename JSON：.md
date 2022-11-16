

# cookie、session和token

http协议本身是一种无状态的协议，一个普通的请求大致分为三步：

1、客户端发送请求给服务器 

2、服务器处理该请求

3、服务器将处理结果响应该客户端。

之后该客户端再次向该服务区发送请求后，服务器端并不能知道这两个请求是否是同一个浏览器或用户发出来的。所以作为web服务器必须能够采用某种方式来唯一识别同一个用户，并记录该用户的状态。而这同一个客户端与服务器端的在一段时间内的多次交互，我们就可以称该客户端为该服务器端的一个客户端会话窗口，有了会话窗口，我们就能确定哪个请求是哪个用户发出的了，从而可以实现会话跟踪，并记录用户的行为。

## 概念：

会话：可以理解为用户打开浏览器，访问该web服务器的多个资源，然后关闭浏览器，这中间的一系列过程称之为一个会话。

有状态的会话：浏览器发送的每一次请求，每一个会话都要有唯一的标识来唯一标识自己，当浏览器发送请求的时候就带上这个标识来让服务器识别，从而实现有“状态”的会话。

javaweb中有两种实现会话的机制：

1）Cookie机制

2）Session机制

# request.getRequestURL()和request.getRequestURI()

 request.getRequestURL() 返回全路径

 request.getRequestURI() 返回除去host（域名或者ip）部分的路径

 request.getContextPath() 返回工程名部分，如果工程映射为/，此处返回则为空

 request.getServletPath() 返回除去host和工程名部分的路径

 例如：
 request.getRequestURL() http://localhost:8080/jqueryLearn/resources/request.jsp 
 request.getRequestURI() /jqueryLearn/resources/request.jsp
 request.getContextPath()/jqueryLearn 
 request.getServletPath()/resources/request.jsp


# **JSON：**

  一种数据格式。



**JSON**和**javascript**对象的相互转换

JSON字符串转换为JavaScript对象使用 JSON.parse()的方法:

```javascript
var obj = JSON.parse(json);
```



JavaScript对象转换为JSON对象使用stringify（）方法:

```javascript

var json = JSON.stringify(user);//字符串{"name":"李老八","age":33,"gender":"男"}
```



Fastjson （由阿里提供的fastjson.jar包）

<!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.76</version>
</dependency>

https://www.runoob.com/w3cnote/fastjson-intro.html



# AJAX

ajax 是一种无须从新加载整个网页的情况下，能够更新部分网页的技术

### $.ajax

```javascript
$.ajax({
    type:"GET 或 POST",
    async: boolen, //因为ajax是异步执行，所以无法直接给全局变量赋值，要把async改为false，变为同步才可以赋值
    url:"规定发送请求的 URL。默认是当前页面。",
    data:{规定要发送到服务器的数据。},
    dataType:"json" a返回的数据如果满足JSON格式，就会自动转为JSON对象 
    success:当请求成功时运行的函数,
    error:如果请求失败要运行的函数,
})
```

### $.get()

```javascript
$.get("URL",data/*键值对格式，传递给后台的数据*/,function(data){
    //拿到返回值做出的操作
} )
```



### $.post()

```javascript
$.post("URL",data/*键值对格式，传递给后台的数据*/,function(data){
    //拿到返回值做出的操作
} )
```

### $.getJSON()





**Java*序列化*就是指把Java对象转换为字节序列的过程  Java*反序列化*就是指把字节序列恢复为Java对象的过程。**

## 异步无刷新请求



ajax的json乱码解决

```xml
<mvc:annotation-driven>
    <!-- 指定http返回编码格式，不然返回ajax请求json会出现中文乱码 -->
    <mvc:message-converters>
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <property name="supportedMediaTypes">
                <list>
                    <value>text/html;charset=UTF-8</value>
                    <value>application/json;charset=UTF-8</value>
                    <value>*/*;charset=UTF-8</value>
                </list>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```





# 拦截器

##      springMVC才有，HandlerInterceptor接口实现



```java
//拦截器
public class Myinterceptor implements HandlerInterceptor {
    //return true 放行
    //return false 不放心
    boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)throws Exception {
        return true;
    }
}
```

##   拦截器配置

```xml

<!--拦截器配置-->
<mvc:interceptors>
    <mvc:interceptor>
        <!--*包括当前路径下的某一个子路径  **是当前路径下的所有路径，包括子路径的子路径-->
        <mvc:mapping path="/**"/>
        <bean id="interceptor" class="com.deng.config.Myinterceptor"/>
    </mvc:interceptor>
</mvc:interceptors>
```

