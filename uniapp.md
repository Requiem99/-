# uniapp

## uniapp各种路由与页面跳转路径

```vue
//跳转有长度限制过长的时候用encodeURIComponent
//pages/test/test?item='+ encodeURIComponent(JSON.stringify(item))
//1.保留当前B页面，跳转到应用内的某个页面,会计入栈中
uni.navigateTo({
    url: 'url?id=1&name=uniapp'
});

//2.关闭当前B页面，跳转到应用内的某个页面,不会计入栈中
uni.redirectTo({
    url: 'url?id=1&name=uniapp'
});

//3.关闭当前所有页面，打开到应用内的某个页面,不会计入栈中
uni.reLaunch({
    url: 'url?id=1&name=uniapp'
});

//4.跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面,不会计入栈中
uni.switchTab({
    url: 'url?id=1&name=uniapp'
});

//5.关闭当前页面，返回上一页面或多级页面,不会计入栈中，
uni.navigateBack({
    delta: 1
});

//6.预加载页面，是一种性能优化技术。被预载的页面，在打开时速度更快
uni.preloadPage({
    url: 'url?id=1&name=uniapp'
});

```

### js文件的引入

![image-20220524111252682](E:image\image-20220524111252682.png)

### 跳转的绝对路径和相对路径

#### 相对路径

​		跳转的相对路径是**以显示的页面(组件)的url来决定**的，例如：显示的父组件为 index.vue ,在index中有引入show1组件，那么在show1组件中要跳转的相对路径就要写相对于 index 组件的相对路径，如果show1组件中还有show2组件，其跳转的相对路径依然是对 index 的相对路径。如果show1，不止用在index组件上展示，在其他组件上导致路径错误，就使用**绝对路径**

#### 绝对路径

​		如果是页面跳转的绝对路径就用 “ /page ” 开头。后不用加 .vue 后缀

![image-20220524112011092](E:image\image-20220524112011092.png)

## 生命周期

![在这里插入图片描述](https://img-blog.csdnimg.cn/219ba7b308784b9296843aa6833f5830.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YWs5a2Z5YWD5LqM,size_20,color_FFFFFF,t_70,g_se,x_16)
onLoad:页面加载了，在onLoad中发送请求是比较合适的，即页面一加载就发送请求获取数据。

onShow:页面显示了，会触发多次，只要页面隐藏，然后再显示出来都会触发。这里会重复触发，如果你重复发送请求不合适。

onReady:页面初次渲染完成了，但是渲染完成了，你才发送请求获取数据，就太慢了。

综上分析，uni-app首页获取**轮播图的请求**应该在onLoad中进行。



# [小程序页面间通信——EventChannel（数据量多时）](https://segmentfault.com/a/1190000040999667)

> 场景：页面 A 跳转 B，需要带一些参数过去，体积小的参数可以通过query带过去，数据量较多时，query不是一个好的选择。这时候应采用小程序的 [EventChannel](https://link.segmentfault.com/?enc=PEHXvDFnE2gWJG4969YBVg%3D%3D.4VFZmBIG2r%2FHLhV55igSuad89%2FkVEuO1bL8icnnmI3mN1c0PBTF6s4LKrkecWeWD69VZQ4l3ELhrv2G0KCZqSUNt4bvA0e7Qwrytnc15aso%3D)

### 一、理论前提

如果一个页面由另一个页面通过 `wx.navigateTo` 打开，这两个页面间将建立一条数据通道：

- 被打开的页面可以通过 `this.getOpenerEventChannel()` 方法来获得一个 `EventChannel` 对象；
- wx.navigateTo 的 `success` 回调中也包含一个 EventChannel 对象。
- 这两个 EventChannel 对象间可以使用 `emit` 和 `on` 方法相互发送、监听事件。

### 二、简单使用：单向传值

#### A页面：

```xml
<button bindtap="navigateToB">跳转B页面</button>
navigateToB () {
  wx.navigateTo({
    url: '/pages/logs/logs',
    success: (res) => {
      // 跳转成功后，触发事件'delivery', 并可携带数据（即第一个参数是事件名，第二个参数是数据包）
      res.eventChannel.emit('delivery', {
          data: '123'
        })
      }
  })
}
```

#### B页面：

```javascript
onLoad() {
  // 建立数据通道
  const eventChannel = this.getOpenerEventChannel()
  // 监听'delivery'事件，并获取数据包
  eventChannel.on('delivery', data => {
    console.log('on - delivery', data)
  })
}
```

通过打印结果看到，数据已经接收

![在这里插入图片描述](https://segmentfault.com/img/remote/1460000040999669)

### 三、双向通信

#### 跳转到B页面后，如果你还需要 **回传一些数据给到A页面**：

- 在B页面中仍然以 `emit` 触发事件，并发送数据包；多个事件平铺
- A页面 `wx.navigateTo` 中的 `events` 参数：是页面间通信接口，用于监听**被打开页面发送到当前页面**的数据

#### A页面：

```javascript
navigateToB () {
  wx.navigateTo({
    url: '/pages/logs/logs',
    // events 用于监听下一个页面的事件 及 回传的数据包
    events: {
      reverseData: (data) => {
        console.log('reverseData', data)
      },
      backData: (data) => {
        console.log('backData', data)
      }
    },
    success: (res) => {
      // 跳转成功后，触发事件'delivery', 并可携带数据（即第一个参数是事件名，第二个参数是数据包）
      res.eventChannel.emit('delivery', {
          data: '123'
        })
      }
  })
}
```

#### B页面：

```javascript
onLoad() {
  // 建立数据通道
  const eventChannel = this.getOpenerEventChannel()
  // 监听'delivery'事件，并获取数据包
  eventChannel.on('delivery', data => {
    console.log('on - delivery', data)
  })
  
  // 通过emit向上一个页面回传数据，多个事件平铺
  eventChannel.emit('reverseData', {
    data: '321'
  })
  eventChannel.emit('backData', {
    data: 'abc'
  })
}
```

然后就可以愉快地传大的数据包了hhhh



# [【uni-app】 onload 和 onshow 的区别](https://www.cnblogs.com/hellocd/p/13998360.html)

## 一、onLoad

只加载一次，监听页面加载，其参数为上个页面传递的数据，参数类型为Object（用于页面传参）

## 二、onShow

监听页面显示。页面每次出现在屏幕上都触发，包括从下级页面点返回露出当前页面。

### 主要区别：

从二级页面返回该页面时，onLoad不会再次加载，而onshow会重新加载。

这点很重要：

1.如果加载列表页，二级页面对一级的列表页面内容有修改，则以及列表函数应该在onShow中加载，否则可以选择onLoad。

2.如果从一个页面携带参数跳转到另外一个页面，在另一个页面获取参数的方式： onLoad(options){ console.log(options.xxx) },这些参数都挂在在options.

总结一下 1：在一些数据变化较少的时候我们用onload 2：像这些order订单列表数据变化及时性我们用的是onshow;

### 总结：

- onLoad先于onShow执行
- onLoad页面的整个生命周期里，只执行一次
- onShow页面的整个生命周期里，可执行多次，即每次显示都会执行
- 获取参数并且只请求一次的事件放在 onLoad 里。
- 当前页面需要时时刷数据的请求多次的事件放在 onShow 里。

## uni-app 子组件中onLoad、onShow里的方法不执行

原因：在[uniapp](https://so.csdn.net/so/search?q=uniapp&spm=1001.2101.3001.7020)中，只有应用生命周期和页面生命周期，子组件是没有应用周期的。所以`onLoad`、`onShow`都不存在。

方法：如果想在渲染子组件时运行一些方法，可以用vue自身的[生命周期](https://so.csdn.net/so/search?q=生命周期&spm=1001.2101.3001.7020)`mounted`