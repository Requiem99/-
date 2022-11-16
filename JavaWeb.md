# JavaWeb

## Http

​    **http**:超文本传输协议（Hyper Text Transfer Protocol，HTTP）是一个简单的请求-响应协议，它通常运行在[TCP](https://baike.baidu.com/item/TCP/33012)之上。它指定了客户端可能发送给服务器什么样的消息以及得到什么样的响应。

超文本：视频，音乐，图片，定位.....

端口：80

  **https**：HTTPS 在HTTP 的基础下加入[SSL](https://baike.baidu.com/item/SSL/320778)，HTTPS 的安全基础是 SSL，因此加密的详细内容就需要 SSL。

端口：443



## 常见状态码

### 200 OK

请求已成功，请求所希望的响应头或数据体将随此响应返回。出现此状态码是表示正常状态。



### 3** 

重定向



### 404 Not Found

请求失败，请求所希望得到的资源未被在服务器上发现。没有信息能够告诉用户这个状况到底是暂时的还是永久的。假如服务器知道情况的话，应当使用410状态码来告知旧资源因为某些内部的配置机制问题，已经永久的不可用，而且没有任何可以跳转的地址。404这个状态码被广泛应用于当服务器不想揭示到底为何请求被拒绝或者没有其他适合的响应可用的情况下。出现这个错误的最有可能的原因是服务器端没有这个页面。



### 5/6开头

服务器错误

## 面试题

**在浏览器输入URL回车之后发生了什么**

https://www.cnblogs.com/jin-zhe/p/11586327.html

## 1、servlet

- servlet是javaEE的规范之一，规范就是接口

- servlet是javaWeb三大组件之一。三大组件分别是：servlet程序，Filter过滤器，Listener监听器

- servlet是运行在服务器上的java小程序，它可以接收客户端发过来的请求，并相应数据给客户端。

  

Servlet（Server Applet），全称Java Servlet，未有中文译文。是用Java编写的服务器端程序。其主要功能在于交互式地浏览和修改数据，生成动态Web内容。

servlet是运行在**Web服务器**或**应用服务器上的**程序，它是作为来自Web浏览器或其他Http客户端的请求和Http服务器上的数据库或应用程序之间的中间层。

狭义的Servlet是指Java语言实现的一个接口，广义的Servlet是指任何实现了这个Servlet接口的类，一般情况下，人们将Servlet理解为后者。

## 2、HelloServlet

编写一个servlet接口，可以直接继承HttpServlet接口



## 3、Request和Response



## 4、Cookie和Session

**会话**：可以理解为用户打开浏览器，访问该web服务器的多个资源，然后关闭浏览器，这中间的一系列过程称之为一个会话。

有状态的会话：浏览器发送的每一次请求，每一个会话都要有唯一的标识来唯一标识自己，当浏览器发送请求的时候就带上这个标识来让服务器识别，从而实现有“状态”的会话。

javaweb中有两种实现会话的机制：

1）Cookie机制  **Cookie**客户端技术

2）Session机制   **Session**服务器技术



### Session

- 服务器会给每一个用户（浏览器）创建一个Session对象；
- 一个Session独占一个浏览器，只要浏览器没有关闭，这个Session就存在；

## 四大作用域

1. ### application (ServletContext）

   1、生命周期：当Web应用被加载进容器时创建代表整个web应用的application对象，当服务器关闭或Web应用被移除时，application对象跟着销毁。  
   2、作用范围：整个Web应用。
   3、作用：   
     a)application.setAttribute(“key”,Object value):存储整个web应用公用的数据
   b)在不同Servlet 之间转发（不常用）    
   this.getServletContext().getRequestDispatcher("/servlet/Demo10Servlet").forward(request,response);   
   　方法执行结束，service就会返回到服务器，再有服务器去调用目标servlet，其中request会重新创建，并将之前的request的数据拷贝进去。      
   **注意**：由于request对象也有getRequestDispatcher("**")方法，所有我们开发是通常使用request调用该方法实现转发。

2. ### session 域 (HttpSession)

   HttpSession 在服务器中，为浏览器创建独一无二的内存空间，在其中保存会话相关的信息。
   　　1、生命周期：在第一次调用 request.getSession() 方法时，服务器会检查是否已经有对应的session,如果没有就在内存中创建一个session并返回。   
   当一段时间内session没有被使用（默认为30分钟），则服务器会销毁该session。 如果服务器非正常关闭（强行关闭），没有到期的session也会跟着销毁。 如果调用session提
   供的invalidate（） ，可以立即销毁session。
   　　**注意**：服务器正常关闭，再启动，Session对象会进行钝化和活化操作。同时如果服务器钝化的时间在session 默认销毁时间之内，则活化后session还是存在的。否则Session不存在。如果JavaBean 数据在session钝化时，没有实现Serializable 则当Session活化时，会消失。

     2、作用范围：一次会话。(用户打开一个浏览器到关闭浏览器可以理解为一次会话)  
     3、作用：保存登录的用户信息、购物车信息等

3. ## request域 (HttpServletRequest)

   ​    1、生命周期：在service 方法调用前由服务器创建，传入service方法。整个请求结束，request生命结束。  
   　2、作用范围：整个请求链（请求转发也存在）。  
   　3、作用：  在整个请求链中共享数据。最常用到：在Servlet 中处理好的数据交给Jsp显示，此时参数就可以放置在Request域中带过去。

4. ## pageContext域(PageContext)

   ​    1、生命周期：当对JSP的请求时开始，当响应结束时销毁。  
   　2、作用范围：整个JSP页面，是四大作用域中最小的一个。  
    作用：  
    （1）获取其它八大隐式对象，可以认为是一个入口对象。  
    （2）获取其所有域中的数据  

**注意**：pageContext 当前页的pageContext对象 ：
**${pageContext.request.contextPath}**返回的是**request.getContextPath()**的值，我们经常使用这个来拼接jsp中的绝对路径。
这里的${pageContext.request.contextPath}是一种特殊用法，不能使用${request.contextPath}的形式替代。





## MVC模式

M ：Model (模型)

 V ：Viwe (视图)

 C : Controller (控制)

**什么是MVC？**

MVC全名是Model View Controller，是模型(model)－视图(view)－控制器(controller)的缩写，一种软件设计典范。

用一种业务逻辑、数据、界面显示分离的方法，将业务逻辑聚集到一个部件里面，在改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑。MVC被独特的发展起来用于映射传统的输入、处理和输出功能在一个逻辑的图形化用户界面的结构中。

Model（模型）是应用程序中用于处理应用程序数据逻辑的部分。
　　通常模型对象负责在数据库中存取数据。
View（视图）是应用程序中处理数据显示的部分。
　　通常视图是依据模型数据创建的。
Controller（控制器）是应用程序中处理用户交互的部分。
　　通常控制器负责从视图读取数据，控制用户输入，并向模型发送数据。

MVC是一个框架模式，它强制性的使应用程序的输入、处理和输出分开。使用MVC应用程序被分成三个核心部件：模型、视图、控制器。它们各自处理自己的任务。最典型的MVC就是JSP + servlet + javabean的模式
Model:常用javabean去实现，通过各种类来对数据库的数据进行获取，并封装在对象当中。

View:常用JSP来实现，通过可直接观察的JSP页面来展示我们从数据库中获取的数据。

Controller:常用servlet来实现，通过servlet来获取经过javabean包装过的对象（已存入数据库中的数据），然后再发送数据传输到JSP界面。



## RestFul风格

get理解为查询  delete就是删除  post就是新增  put就是更新数据