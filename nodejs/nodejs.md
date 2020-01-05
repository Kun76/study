# node.js的模块化

## fs模块

fs模块是文件操作模块。

### 异步格式:

readFile读取文件

```js
// 引入模块
const fs = require('fs');

// 调用readFile进行文件内容获取
/* 参数1,地址
    参数2,可选,一般设置uft8;如果没有设置那么返回buffer格式,二进制
    参数3,回调函数,俩个参数, 如果出错,第一个为错误对象,第二个为undefined,如果正确第一个为null,第二个为内容
*/
fs.readFile('./demo.js', 'utf8', (err, data) => {
    if (err) {throw err;}//throw称为抛异常,有异常会报错
    console.log(data);
})
```

### 同步格式 :

readFileSync

```js
// 引入模块
const fs = require('fs');

// 两个参数,参数一必填 地址,参数可选,utf8
// var data = fs.readFileSync('demo.js', 'utf8');
// var data = fs.readFileSync('demo.js');
// console.log(data);
// console.log(data.toString());
// 返回值:成功时是读取内容;如果出现错误会报错导致终止

// 异常捕获:当我们不希望代码出现报错的一种手段; try catch
try {
    // 这里放置可能会报错的代码
    var data = fs.readFileSync('demo.js', 'utf8');
    console.log(data);
} catch (error) {
    console.log("文件读取有误,请检查地址后重试");
    // console.log(error);error是错误的详细信息,如果不想使用可以去除,包括catch后面的
}
```

与异步格式不同于:

- api的名字后面有Sync
- 不是通过回调函数来获取值,而是像一个普通的函数调用一样,直接获取返回值

#### 附：fs模块中的常用方法

| API                                         | 作用              | 备注           |
| ------------------------------------------- | ----------------- | -------------- |
| fs.access(path, callback)                   | 判断路径是否存在  |                |
| fs.appendFile(file, data, callback)         | 向文件中追加内容  |                |
| fs.copyFile(src, callback)                  | 复制文件          |                |
| fs.mkdir(path, callback)                    | 创建目录          |                |
| fs.readDir(path, callback)                  | 读取目录列表      |                |
| fs.rename(oldPath, newPath, callback)       | 重命名文件/目录   |                |
| fs.rmdir(path, callback)                    | 删除目录          | 只能删除空目录 |
| fs.stat(path, callback)                     | 获取文件/目录信息 |                |
| fs.unlink(path, callback)                   | 删除文件          |                |
| fs.watch(filename[, options]\[, listener])  | 监视文件/目录     |                |
| fs.watchFile(filename[, options], listener) | 监视文件          |                |
| fs.existsSync(absolutePath)                 | 判断路径是否存在  |                |

### 路径问题

绝对路径： 从磁盘根目录开始到指定文件的路径。

相对路径：是以某个文件的位置为起点，相对于这个位置来找另一个文件。

* __dirname：获取当前被执行的文件的文件夹所处的绝对路径
* __filename：获取当前被执行的文件的绝对路径

## path模块

核心模块,用来处理路径问题;

- path.basename 获取返回 path 的最后一部分(文件名)
- path.dirname 返回目录名
- path.extname 返回路径中文件的扩展名(包含.)

```js
// 引入模块path
const path = require('path');
//返回set.html
path.basename('C:\Users\yang1\Desktop\就业班\nodejs\es6\set.html');
//返回C:\Users\yang1\Desktop\就业班\nodejs\es6
path.dirname('C:\Users\yang1\Desktop\就业班\nodejs\es6\set.html');
path.extname('index.html');// 返回: '.html'
```

- **path.join()**  （常用）拼接路径

```js
path.join(__dirname, '/path.js');
//获取当前文件的绝对路径
```

* path.parse(  ) 把路径字符串解析成对象的格式

```js
path.parse('C:\Users\yang1\Desktop\就业班\nodejs\es6\set.html') 
//返回一个对象
{
  root: 'C:\\',
  dir: 'C:\\Users\\yang1\\Desktop\\就业班\\nodejs\\es6',
  base: 'set.html',
  ext: '.html',
  name: 'set'
}
```

### 附：path模块其它方法列表

| 方法                       | 作用                               |
| -------------------------- | ---------------------------------- |
| path.basename(path[, ext]) | 获取返回 path 的最后一部分(文件名) |
| path.dirname(path)         | 返回目录名                         |
| path.extname(path)         | 返回路径中文件的扩展名(包含.)      |
| path.format(pathObject)    | 将一个对象格式化为一个路径字符串   |
| path.join([...paths])      | 拼接路径                           |
| path.parse(path)           | 把路径字符串解析成对象的格式       |
| path.resolve([...paths])   | 基于当前工作目录拼接路径           |



## http模块

http是nodejs的核心模块，让我们能够通过简单的代码创建一个Web服务器，处理http请求。

```js
//1.引入核心模块http
const http = require('http');
//2.调用方法创建web服务器实例(可以创建很多个实例);
//参数1 req 请求的相关功能组成的对象
//参数2 res 响应的相关功能组成的对象
http.createServer((req,res) => {
    // 在响应之前给响应头设置格式,准确告诉客服端响应数据的格式
    res.setHeader('Content-Type','text/html;charset=utf-8');
    console.log(req.connection.remoteAddress); // 娱乐用的，可以查看访问者的ip
    //res.end(响应内容)每个服务器的代码必须以res.end()结尾,否则响应无法发送
    res.end()
    //调用web服务器实例的listen()监听某个端口
}).listen(8000,()=>{
    console.log('监听8000成功 可以访问')
});
```

###请求和响应

通过http.createServer方法创建一个http服务,他有两个参数;第一个参数表示`来自客户端浏览器的请求`，第二个参数用来`设置对本次请求的响应`。

* req：第一个参数中包括本次请求的信息
  * **req.url**。本次请求的地址
  * **req.method**。   获取请求行中的请求方法
  * **req.headers**。    获取请求头
* res : 第二个参数用来设置本服务器对这次请求的处理。
  * 这个参数是一个对象,有很多方法和属性
  * **res.end()** 把本次的处理结果返回给客户端浏览器,如果不写,则客户端浏览器永远收不到响应
  * **res.setHeader()**设置响应头,比如设置响应体的编码`res.setHeader('content-type', 'text/html;charset=utf-8');`
  * **res.statusCode** 设置状态码

###根据不同的url处理不同的请求

```js
// http.js
// 引入核心模块http
const http = require('http');

// 创建服务
const server = http.createServer(function(req, res) {
  if(req.url === "/a.html"){
      // 读出文件内容
      // 通过res.end()返回
  }
  else if(req.url === "/b.html"){
      
  }
    else{
        res.end("");
    }
});
// 启动服务
server.listen(8081, function() {
  console.log('success');
});
```

###处理静态资源

静态资源指的是html文件中链接的外部资源,如css,js,img文件等

```js
// 引入模块,创建一个web服务器,能够进行静态资源展示
const fs = require('fs');
const path = require('path');
const http = require('http');
// 创建一个web服务器
const app = http.createServer((req, res) => {

  if (req.url === '/index.html') {
    let htmlString = fs.readFileSync(path.join(__dirname, 'index.html'));
    res.end(htmlString);
  }
  else if (req.url === '/style.css') {
    let cssString = fs.readFileSync(path.join(__dirname, 'style.css'));
    res.setHeader('content-type', 'text/css');
    res.end(cssString);
  } else if (req.url === '/1.png') {
    let pngString = fs.readFileSync(path.join(__dirname, '/1.png'));
    res.end(pngString);
  } else {
    res.setHeader('content-type', 'text/html;charset=utf-8');
    res.statusCode = 404;
    res.end('<h2>可惜了, 找不到你要的资源' + req.url + '</h2>');
  }
}); 
//启动服务器，监听8082端口
app.listen(8082, () => {
  console.log('8082端口启动');
});
```

### 为不同的文件类型设置不同的 Content-Type

通过使用res对象中的setHeader方法，我们可以设置content-type这个响应头。这个响应头的作用是告诉浏览器，本次响应的内容是什么格式的内容。以方便浏览器进行处理。

常见的几中文件类型及content-type如下。

- .html：` res.setHeader('content-type', 'text/html;charset=utf-8') `
- .css：`res.setHeader('content-type', 'text/css;charset=utf-8')`
- .js：`res.setHeader('content-type', 'application/javascript') `
- .png：`res.setHeader('content-type', 'image/png')`

其它类型，参考这里：https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types

###静态资源托管

​        之前我们进行了基本的静态资源处理,实现方式就是判断每个文件请求时的url，并进行具体判断和处理即可,问题是文件较多时，书写较为繁琐，可以通过某些操作进行统一处理

  \- 这种统一的处理方式称为静态资源托管

```js
// 1 引入相关模块
const http = require('http');
const fs = require('fs');
const path = require('path');

// 2 创建一个服务器实例
http.createServer((req, res) => {
  // 3 对请求进行处理
  const reqUrl = req.url;

  // 3.1 进行首页检测
  if (reqUrl === '/' || reqUrl === '/index.html') {
    let tempPath = path.join(__dirname, './public/index.html');
    var data = fs.readFileSync(tempPath);
    // 3.1.1 设置响应头
    res.setHeader('Content-Type', 'text/html;charset=utf-8');
    // 3.1.2 设置响应体
    res.end(data);

    // 4 静态资源托管处理
    //  4.1 检测是否为静态资源：都以/public/开头
  } else if (reqUrl.startsWith('/public/')) {
    // 4.2 获取当前请求的静态资源的后缀，进行响应头的设置
    const ext = path.extname(reqUrl);
    let mimeType;
    switch (ext) {
      case '.css':
        mimeType = 'text/css';
        break;
      case '.js':
        mimeType = 'application/javascript';
        break;
      case '.jpg':
        mimeType = 'image/jpeg';
        break;
    }
    res.setHeader('Content-Type', mimeType + ';charset=utf-8');

    // 4.3 根据url读取文件内容
    let tempPath = path.join(__dirname, reqUrl);
    let data = fs.readFileSync(tempPath);
    res.end(data);
  } else {
    res.end('ok');
  }
})
  .listen(8000, () => {
    console.log('监听8000端口成功...');
  });
```

###get接口设置

```js
// 1 引入http模块
const http = require('http');
// node中有一个核心模块叫做url,用于进行url地址处理
//  - 引入url模块
const url = require('url');

// 2 创建web服务器实例
http.createServer((req, res) => {
  // req.method 请求方式 (大写名称，判断时需要注意)
  // req.url 请求地址

  // - req.url除了含有接口地址，还可能含有请求参数
  //   - 使用url.parse()将req.url处理为多部份的形式(对象结构)
  //    - 返回值是将地址处理得到的对象结构
  //      - pathname 路径名称，就是我们需要的接口名（不含参数）
  //      - query 请求参数，不含?部分，是url编码格式
  //        - url.parse()传入参数2为true时，query的值为对象结构(常用方式)
  // console.log(req.url);
  // console.log(url.parse(req.url));
  // console.log(url.parse(req.url, true));

  // - 声明变量保存我们需要使用的两个属性
  let { pathname, query } = url.parse(req.url, true);
  if (pathname === '/admin/getCate' && req.method === 'GET') {
    // - 根据需求进行响应处理即可
    //   - 例如，设置模拟响应内容，根据用户的id筛选部分数据，在传递一个时间戳和status和msg即可
    //   - 设置假数据(不是get接口设置的必要代码，可根据需求进行其他处理)
    const datas = [
      { id: 1, name: 'jack', age: 18 },
      { id: 2, name: 'rose', age: 21 },
      { id: 3, name: 'lily', age: 12 },
      { id: 4, name: 'lucy', age: 32 }
    ];
    var resData = datas.filter((ele) => {
      return ele.id === +query.id
    });
    // - 例如，希望给响应数据中添加一些信息
    resData = {
      status: 200,
      msg: 'success',
      data: resData,
      time: +new Date()
    };
    // 将数据响应给客户端
    //  - 设置响应头告诉客户端响应的数据是JSON格式
    res.setHeader('Content-Type', 'application/json;charset=utf-8');
    //  - 将数据转换为JSON格式
    res.end(JSON.stringify(resData));
  } else {
    res.end('Not Found');
  }
})
  // 用来监听端口
  .listen(8000, () => {
    console.log('监听8000端口成功...');
  });
```

### post接口设置

```js
// 1 引入http模块
const http = require('http');
// - node中提供了一个核心模块querystring，用来对url编码格式的数据进行转换处理
//    - 可以将url编码格式的字符串转换为对象结构
//    - 之前使用的url.parse()参数2，就是让url调用了querystring进行处理
// - 引入querystring模块
const querystring = require('querystring');

// 2 创建web服务器实例
http.createServer((req, res) => {
  // 3 进行post请求的处理操作
  //  3.1 判断请求地址和请求类型
  if (req.url === '/admin/addCate' && req.method === 'POST') {
    // 3.2 接收post请求参数
    //   - 给req设置两个事件 data end
    let dataStr = '';
    req.on('data', (tempData) => {
      dataStr += tempData;
    });

    req.on('end', () => {
      // 在end中使用接收完毕的所有请求参数
      // console.log(dataStr);

      // 调用querystring模块的parse()将dataStr转换为对象
      let dataObj = querystring.parse(dataStr);
      // console.log(dataObj);

      // 根据具体需求进行数据处理，处理后进行响应即可
      res.setHeader('Content-Type', 'application/json;charset=utf-8');
      res.end(JSON.stringify({
        name: 'jack',
        age: 18
      }));
    });

  }
}).listen(8000, () => { console.log('监听8000...'); })
```

### 传输数据接口设置

```js
// 1 引入http，fs，path，querystring模块
const http = require('http');
const fs = require('fs');
const path = require('path');
const querystring = require('querystring');
// 2 创建一个web服务器
http.createServer((req, res) => {
  //  - 设置一个POST请求的接口，接口名为 /addCate
  //    - 两个请求参数必须传，name,age
  //    - 将传递的请求参数设置为对象结构保存在category.json文件中即可
  //      - fs.writeFile('地址', '内容', 回调函数);
  //      - fs.writeFileSync('地址', '内容');
  //        - 写入时旧的内容会被覆盖
  //    - 处理方式：
  //      - 1 读取数据 2 在读取的数据中添加新数据 3 写入到文件中即可

  // 3 进行接口判断
  if (req.url === '/addCate' && req.method === 'POST') {
    // 4 设置事件接收请求参数
    let dataStr = '';
    req.on('data', (tempData) => {
      dataStr += tempData;
    });
    req.on('end', () => {
      // 5 接收请求参数，并转换为对象结构
      let dataObj = querystring.parse(dataStr);

      // 6 添加id属性(设置一个随机的id值即可)
      //   - 设置时间戳 + 随机数的处理方式比较保险，因为书写较长，此处采用简略写法
      dataObj.id = Math.random().toString().slice(2);

      // 7 读取文件
      let tempPath = path.join(__dirname, './category.json');
      let jsonStr = fs.readFileSync(tempPath, 'utf8');
      // 7.1 将读取的json文件内容转换为对象
      let jsonObj = JSON.parse(jsonStr);

      // 8 将新数据添加到文件数据中
      jsonObj.push(dataObj);

      // 9 写入到文件中即可（记得进行JSON转换)
      fs.writeFileSync(tempPath, JSON.stringify(jsonObj));

      // 10 进行响应处理即可
      res.setHeader('Content-Type', 'application/json;charset=utf-8');
      res.end(JSON.stringify({
        status: 200,
        msg: 'success'
      }));
    });
  }
}).listen(8000, () => { console.log('监听8000...'); })
```

