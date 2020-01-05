# 原生Ajax使用

## get请求基本使用

- **步骤：**

  1. 创建xhr对象

  2. 调用open：设置请求方式和请求地址(第三个参数为默认值异步,true,设置false则是同步)

  3. 调用send: 发送请求（这一步是异步操作）

  4. 设置onreadystatechange事件，监听请求的各种状态

     - readyState是xhr的属性，用来表示请求发送的状态
       - 取值为4代表下载完毕

       - 必须确保readyState为4才能使用响应的数据

     - xhr.status 代表请求是否成功

       - 200代表请求是成功的

     - xhr.responseText 代表接收的响应内容

```js
// 1 创建xhr对象
    
    // 2 调用open：设置请求方式和请求地址
    xhr.open('GET', 'http://www.liulongbin.top:3006/api/getbooks');
    // 3 调用send: 发送请求，这一步是异步操作
    xhr.send();

    // 4 设置事件，监听请求的各种状态
    //   - readyState是xhr的属性，用来表示请求发送的状态
    //     - 取值为4代表下载完毕
    //     - 必须确保readyState为4才能使用响应的数据
    //     - 进一步检测：
    //       - xhr.status 代表请求是否成功
    //          - 200代表请求是成功的
    xhr.onreadystatechange = function () {
      // 4.1 检测xhr.readyState取值和xhr.status取值
      if (xhr.readyState === 4 && xhr.status === 200) {
        // 4.2 接收响应的数据即可
        //    - 原生接收的响应内容是JSON格式，需要自己进行转换
        //      - jQuery会自动转换
        console.log(xhr.responseText);
      }
    };
```

## 带有参数的get请求

- 书写方式：
  - **位置：在xhr.open()的参数2 url后面书写参数内容**
  - 名称：参数的形式称为**查询字符串**
  - 格式：  ?名=值&名=值...
    - 其中  名=值&名=值...  称为**url编码格式，英文为urlencoded**
- jQuery的get请求和原生get请求的**本质是一样**的

```js
// 1 创建xhr对象
    var xhr = new XMLHttpRequest();
    // 2 调用open：设置请求方式和请求地址
    //    - 如果希望设置get请求的请求参数，需要放置在open()参数2地址的最后位置
    //    - 书写方式为：  地址?名=值&名=值....
    //    - 名称说明： 名=值&名=值称为url编码格式  urlencoded
    xhr.open('GET', 'http://www.liulongbin.top:3006/api/getbooks?id=3&name=jack&age=18');
    // 3 调用send: 发送请求，这一步是异步操作
    xhr.send();

    // 4 设置事件，监听请求的各种状态
    xhr.onreadystatechange = function () {
      // 4.1 检测xhr.readyState取值和xhr.status取值
      if (xhr.readyState === 4 && xhr.status === 200) {
        // 4.2 接收响应的数据即可
        console.log(xhr.responseText);
      }
    };

```

## (了解)url编码的说明

- **url编码是url对地址的一种处理方式(将中文部分进行编码转换**)

- 浏览器会进行自动的url处理，无需我们自己操作
- 两个用来处理的函数
  - encodeURI() 编码
  - decodeURI() 解码

## post请求的发送方式

- 步骤：
  1. 创建xhr对象
  2. 调用xhr.open()
  3. 设置Content-Type（内容格式、内容类型）
     - xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')
  4. 调用xhr.send(数据)
     - 数据为url编码格式
  5. 设置readystatechange事件获取响应内容

## get与post发送方式的区别

- **请求参数的书写位置不同：**
  - get请求方式：在xhr.open()的url后面使用?连接
  - post请求：在xhr.send()中书写
- **post请求需要设置Content-Type**

## 封装ajax函数

- 封装用来将对象转换为url编码的函数

```js
// 封装函数，用于将对象转换为url编码格式
    function urlencoded(obj) {
      var arr = []; // 声明数组用来保存结果
      // 遍历obj，获取所有属性
      for (var k in obj) {
        // k - 属性名 - 字符串类型
        // obj[k] - 属性值
        arr.push(k + '=' + obj[k]);
      }
      return arr.join('&');
    }
```

- 封装ajax函数
  - **转换字符串为大小写的字符串方法**
    - 字符串.toUpperCase() 转换为大写字母
    - 字符串.toLowerCase() 转换为小写字母

```js
// 封装用来发送ajax请求的函数
    //  参数：
    //    对象
    //      - type 请求方式
    //      - url 请求地址
    //      - data 请求参数
    //      - success 回调函数
    function ajax(options) {
      // 1 创建xhr对象
      var xhr = new XMLHttpRequest();
      // 2 设置请求的相关信息: 需要根据请求方式分别设置
      //   - 统一保存数据
      var data = urlencoded(options.data);
      //  2.1 将请求方式统一转换为大写(或小写)
      var type = options.type.toUpperCase();
      //  2.2 对请求方式进行检测
      if (type === 'GET') {
        xhr.open('GET', options.url + '?' + data);
        xhr.send();
      } else if (type === 'POST') {
        xhr.open('POST', options.url);
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
        xhr.send(data);
      }
      // 3 设置事件接收响应内容
      xhr.onreadystatechange = function () {
        // 3.1 检测
        if (xhr.readyState === 4 && xhr.status === 200) {
          // 3.2 将JSON格式的字符串转换为对象
          var res = JSON.parse(xhr.responseText);
          options.success(res);
        }
      };
    }


// 进行数据处理的函数
    function urlencoded(obj) {
      var arr = []; // 声明数组用来保存结果
      // 遍历obj，获取所有属性
      for (var k in obj) {
        // k - 属性名 - 字符串类型
        // obj[k] - 属性值
        arr.push(k + '=' + obj[k]);
      }
      return arr.join('&');
    }
```

- 可选：可以给ajax函数的options中的属性设置默认值
  - type默认值为'GET'
  - data默认为''
  - success如果不存在，则不进行调用，避免报错

## ajax level2新功能: 超时时间设置

- 设置方式：
  - 属性：  xhr.timeout = 超时时间; //  毫秒单位
  - 事件：  xhr.ontimeout = function() { 请求超时后，触发的事件 };

## FormData的使用

- 作用：

  - 可以快速处理表单的数据
  - 可以进行文件上传

- **注意点：**

  - **发送FormData需要使用POST请求方式**
  - **不需要单独设置Content-Type**

- 使用方式：

  - 单独发送数据

    ```js
      // 1 空FormData对象，添加数据后发送
          // 1.1 创建FormData对象
          var fd = new FormData(); // 空的FormData对象
          // 1.2 向fd中添加一些数据
          fd.append('name', 'jack');
          fd.append('age', 18);
      
          // 1.3 使用POST方法将fd发送到对应接口（此处的接口为formdata）
          //  - 无需设置之前post的content-type
          var xhr = new XMLHttpRequest();
          xhr.open('POST', 'http://www.liulongbin.top:3006/api/formdata');
          xhr.send(fd); // 将fd放入在send()参数中即可
          xhr.onreadystatechange = function () {
            if (xhr.readyState === 4 && xhr.status === 200) {
              console.log(xhr.responseText);
            }
          };
    ```

  - 处理表单数据

    ```js
      <!-- 设置form标签，用来进行formdata操作 -->
        <form id="testForm">
          <!-- 如果要表单元素数据被请求发送，必须设置name -->
          <input type="text" name="username">
          <input type="password" name="password">
          <!-- 设置按钮，点击后，使用FormData处理表单数据 -->
          <button type="button" id="btn">ajax提交</button>
        </form>
      
      
      // 2 通过FormData对象管理表单元素，再发送(同时也可以添加数据)
          document.getElementById('btn').onclick = function () {
            // 2.1 创建FormData对象，传入DOM对象形式的form标签到参数中
            var testForm = document.getElementById('testForm');
            var fd = new FormData(testForm); // 空的FormData对象
      
            // 2.1.1 也可以在存在form的基础上，自己添加数据
            fd.append('name', 'rose');
            fd.append('age', 21);
      
            // 2.3 使用POST方法将fd发送到对应接口（此处的接口为formdata）
            //  - 无需设置之前post的content-type
            var xhr = new XMLHttpRequest();
            xhr.open('POST', 'http://www.liulongbin.top:3006/api/formdata');
            xhr.send(fd); // 将fd放入在send()参数中即可
            xhr.onreadystatechange = function () {
              if (xhr.readyState === 4 && xhr.status === 200) {
                console.log(xhr.responseText);
              }
            };
          }
    ```

  - 文件上传

    - **1 准备结构(文件域)**

    ```html
    <input type="file">
    ```

    - **2 检测用户是否选择了文件**
      - 对DOM对象形式的input[type="file"]的**files**属性进行检测
        - 如果内部存储了任意的一个值，就说明选取了文件，否则就是没选
    - 3 使用FormData保存文件信息
      - 使用fd.append()添加即可
    - 4 通过ajax发送
    - 5 响应处理，展示服务端获取到的图片

## jQuery使用FormData上传文件的注意点

- 必须设置两个属性
  - contentType: false 不指定内容类型
  - processData: false 不进行数据处理

## 上传进度条功能

- xhr.upload.onprogress 上传中实时触发事件
  - 事件对象e的属性
    - e.lengthComputable 文件长度使用可用
    - e.loaded 以上传大小
    - e.total 总大小
- xhr.upload.onload 上传完毕时触发事件