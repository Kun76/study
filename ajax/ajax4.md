# jQuery的全局ajax处理器

> 作用：用来对页面中的ajax操作进行统一设置，简化操作
>
> 常见场景：加载资源的loading功能

- ajaxStart() 任意请求开始时触发内部回调

  - 写法：

  ```js
  $(document).ajaxStart(function () {
    // 代码...
  });
  ```

- ajaxStop() 任意请求结束后触发内部回调

  - 写法：

  ```js
  $(document).ajaxStop(function () {
    // 代码...
  });
  ```


# axios使用

- axios.get()的使用

```js
// axios.get('地址', {params: {数据。。。}}).then(成功时的处理函数);
// 示例：
 axios.get('http://www.liulongbin.top:3006/api/getbooks', {
      params: {
        id: 2
      }
    })
      .then(function (res) {
        // axios对响应的res部分进行了包装，之前jQuery中的res，相当于axios中的res.data
        //  - 刚开始使用时，可以使用以下操作
        res = res.data;
        console.log(res);
      }); 
```

- axios.post()的使用

```js
// axios.post('地址', {数据。。。}).then(成功时的处理函数)
    axios.post('http://www.liulongbin.top:3006/api/post', {
      name: 'jack',
      age: 18
    })
      .then(function (res) {
        console.log(res);
      });
```

- axios()的使用

  - 发送get和post的区别：

    - **设置请求参数的属性名不同**
      - get请求参数的属性名为params
      - post为data

  - 发送get请求演示：

    ```js
    axios({
          method: 'GET',
          url: 'http://www.liulongbin.top:3006/api/get',
          params: {
            name: 'jack',
            age: 18
          }
        })
          .then(function (res) {
            console.log(res.data);
          });
    ```

  - 发送post请求演示：

    ```js
    axios({
          method: 'POST',
          url: 'http://www.liulongbin.top:3006/api/post',
          data: {
            name: 'rose',
            age: 21
          }
        })
          .then(function (res) {
            console.log(res.data);
          });
    ```

# 跨域

## 同源策略

### 同源(同源地址)

> 指的是两个地址的 **协议 域名 端口** 完全一样，这两个地址就是同源的，或者称为同源地址。

- 我们发现，在现代网站中(京东)一个公司可能具有很多的域名，也都不是同源地址，如果在公司内部都不能进行数据请求，是很不方便的。

### 浏览器同源策略的机制

> 发送跨域请求时，请求发送是成功的，响应也是成功的，只不过浏览器将响应的信息拦截了，我们无法操作。

- 对我们操作的影响，无法对非同源的地址发送ajax请求(不能跨域)

## 跨域方式

- JSONP
  - 兼容性好，但是只支持GET
- CORS - nodejs中学
  - 兼容性不太好，但是支持GET、POST，并且是标准中提供的方式。

## JSONP的实现原理

- 实现原理：
  - 同源策略不限制script标签对非同源地址进行请求
  - 可以借助script标签进行JSONP实现
- 实现方式:

```php+HTML
<!-- 
    使用JSONP进行请求处理
      步骤:
        1 准备一个处理函数
        2 通过script标签的src属性，进行接口的请求
          - 因为同源策略不会限制script标签的功能
        3 请求时传入请求参数
          - callback 用来传递本地处理函数的函数名
          - 其他参数随便写
        4 当响应结束后，步骤1设置的处理函数会自动调用
          - 可以在处理函数中对响应数据进行后续处理
  -->
  <script>
    function fun(res) {
      console.log(res);
    }
  </script>
  <script src="http://ajax.frontend.itheima.net:3006/api/jsonp?callback=fun&name=jack"></script>

```

- 小说明：
  - JSONP和JSON没有本质关联
  - **JSONP与ajax没有任何关联**
    - JSONP是使用script标签进行请求发送
    - ajax是通过浏览器提供的XMLHttpRequest对象发送

## jQuery发送JSONP请求的方式

- jQuery使用的是$.ajax()方法进行JSONP操作
  - **设置dataType属性为'jsonp'**
- 小说明：
  - 可以从浏览器的network看到请求的类型为 script 
  - jQuery会将使用过的用于发送jsonp请求的script标签移除，打开页面也看不到

```js
	$.ajax({
      type: 'GET',
      url: 'http://ajax.frontend.itheima.net:3006/api/jsonp',
      data: {
        name: 'jack',
        age: 18
      },
      /* 
      // 下面的两个属性作为了解即可，通常不会使用
      jsonp: 'xyz', // 将默认的请求参数名callback设置为其他名称
      jsonpCallback: 'abc', // 将处理函数名称设置为指定名称
      */
      dataType: 'jsonp', // 设置这句话后，jQuery就会使用JSONP的方式进行请求发送
      success: function (res) {
        console.log(res);
      }
    });
```

# 节流和防抖

## 防抖

> 进行重复操作时，设置时间间隔，以最后一次满足间隔的操作为准进行执行。
>
> 简单来说：**就是以满足间隔的最后一次为准**
>
> **所谓防抖，就是指触发事件后在 n 秒内函数只能执行一次，如果在 n 秒内又触发了事件，则会重新计算函数执行时间。**

