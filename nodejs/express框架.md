# express

## npm使用

 1 联网

 2 在某个项目目录中打开终端

 3 执行 npm init -y 初始化，会自动创建package.json目录

 4 执行 npm i 包名 进行包的安装，例如 npm i jquery  npm i express

 5 通过 npm uninstall 包名 命令删除包

 6 在已经安装包的情况下手动删除了node_modules目录，可以通过 npm i 命令，自动安装项目依赖的所有包

##.gitigonre

创建.gitigonre文件,可以隐藏文件不上传到git上; 便于上传文件,有package.json的文件的情况下可以直接npm i 下来需要的模块包

##express静态托管

```js
// 第三方模块的引入方式与核心模块相同，使用require('模块名即可')
const express = require('express');
const path = require('path');

// 1 创建一个express实例
const app = express();

// 2 静态资源托管
// app.use(express.static('绝对路径地址'));

// 设置网站的默认静态资源(下面两行效果相同)
app.use(express.static(path.join(__dirname, 'public')));
// app.use('/', express.static(path.join(__dirname, 'public')));

// - 对某个具体url地址，进行静态资源托管
// app.use('url地址', express.static('绝对路径地址'));
    app.use('/admin/abc', express.static(path.join(__dirname, 'admin_public')));

// 3 监听端口
app.listen(8000, () => { console.log('监听了8000...') });
```

想要实现静态资源托管,使用**express.static('绝对路径地址')**

##express中的接口设置

### get

```js
//引入模块文件
const express = require('express');
// 创建express实例
const app = express();
// 设置get请求接口 参数1是接口地址,参数2是回调函数
app.get('/getCate', (req, res) => {
    // req是请求对象,res是响应对象
    // express给req设置了query,是get的请求参数,对象格式
    console.log(req.query);
    // res.end('ok'); express中响应内容除了end还有json和send方式 send什么都能发 ,json可以发送json格式的数据
    // res.json({ name: 'liuneng', age: 43 });
    res.send({ name: 'liuneng', age: 43 });
});
app.listen(8000, () => { console.log('监听8000成功...') })
```

注意事项 : 

* express模块给req设置了一个query属性,值get的请求参数,对象格式\
* express中响应内容除了end还有json和send方式 send什么都能发 ,json可以发送json格式的数据

###post

```js
//引入模块文件
const express = require('express');
// 如果希望post数据处理时,可以使用body-parser包,下载express时同时也下载了,可以直接使用
const bp = require('body-parser');
//创建express实例
const app = express();
// - 使用bp包之前需要先进行配置
//    - 因为bp默认采用的是qs包进行url编码转换
//    - 如果希望使用核心模块进行处理，必须进行以下设置
//    - 下面这句话的含义为：使用核心模块querystring进行处理操作
app.use(bp.urlencoded({ extended: false }));
app.post('/admin/wahaha', (req, res) => {
    // body-parser模块给req添加一个属性body,对象结构的post请求参数
    console.log(req.body);

    res.send('ok');
});
// 设置监听事件
app.listen(8000, () => {
    console.log('监听8000成功');
})
```

注意点 :

* 处理post数据时,可以使用body-parser模块(下载express是同时也下载了),
* app.use(bp.urlencoded({ extended: false })); body-parser默认使用qs包进行url编码转换,设置这句话的意思是使用核心模块querystring进行处理操作

##传输文件multer模块

如果想要传输文件,那么就可以使用multer模块,

```js
// 引入模块
const express = require('express');
// 通过multer模块来进行文件上传处理
const multer = require('multer');
// 设置实例
let app = express();
// 简易方法,意思是保存到当前目录中的uploads中,如果没有文件夹,那么就会自动创建一个文件夹;
let upload = multer({ dest: './uploads' });
app.post('/fileupload', upload.single('file'), (req, res) => {
    //upload.single('file') 设置默认发送文件的名称,表单发送文件是name是file
    // multer给file保存的文件的相关信息
    console.log(req.file);
    res.send('ok');
})
// 设置监听
app.listen(8000, () => {
    console.log('监听成功')
});
```

如果想要使用详细方法设置,那么就使用固定格式

```js
详细方法设置
let upload = multer({
    storage: multer.diskStorage({
        destination: (req, file, callback) => {
            //参数一位错误参数,二为保存地址
            callback(null, './uploads');
        },
        filename: (req, file, callback) => {
            callback(null, Math.random() + file.originalname)
        }
    })
})
```

## express中间件

中间件**是**一个特殊的url地址处理**函数**

```js
//引入模块 模拟body-parser功能
const express = require('express');
const querystring = require('querystring');
// 创建实例
const app = express();
app.use((req, res, next) => {
    if (req.method === 'POST') {
        let datastr = '';
        req.on('data', (tempstr) => {
            datastr += tempstr;
        });
        req.on('end', () => {
            // 转换格式
            let Objstr = querystring.parse(datastr);
            req.body = Objstr;
            next();
        })

    } else {
        next();
    }
});
app.post('/addCate', (req, res) => {
    console.log(req.body);
    res.send('ok');
});
app.get('/getCate', (req, res) => {
    console.log(req.query);
    res.send("ok");
})
app.listen(8000, () => { console.log('监听8000成功') });
```

## 自定义模块

可以自定义模块*module*.*exports*,就跟自动义函数一样,可以创建方法,并且引用

```js
//   - 如果希望当前模块中的某些数据可以被其他模块使用，需要进行特殊操作
//   - 操作方式称为： 暴露 或者 导出

// 导出的方式: (选择其中一种使用即可)
//  - 方式1：(默认方式)
//    - 如果要导出的数据很多时，推荐使用第一种形式
//    - 如果突然分不清两者的关系了，全用第一种肯定没错
// console.log(module.exports);
module.exports = {
  username,
  password,
  userAge,
  sayHi
};

// module.exports.name = 'jack'; // 也可以这样写


//  - 方式2：(简写)
//    - 方式2的exports其实是复制了方式一的module.exports
//    - 如果要导出的属性较少，可以使用第二种方式
// console.log(exports);
/* exports.name = 'jack';
exports.age = 18; */

// 一定不要给exports进行赋值操作，会切断与module.exports的引用
```

module.exports.xx = { }里面是需要的函数体

## 路由中间件

**使用场景**

路由过多时，代码不好管理。以大事件的代码为例，我们定义了管理员角色的接口和普通游客的接口，这些接口如果全写在一个入口文件中(如下只是显示了4个接口，如果是40个接口，就会很难读了)，是很不好维护的。

目的 : 把它们拆开到不同的文件中，以便于管理。

主页代码

```js
//引入模块
const express = require('express');
//创建experss的实例
let app = express();
// 引入用户路由中间件
const userRouter = require('./router/userouter');
// 使用
app.use('/user', userRouter)
app.listen(8000, () => {console.log('监听8000成功')})
```

分类管理

```js
//设置路由中间件
const express = require('express');
//express.Router是模块自带的方法,可以设置中间件
const router = express.Router;
router.get('/get', (req, res) => {
    console.log('/user/get');
    res.send('ok');
})
router.post('/post', (req, res) => {
    console.log('/user/post');
    res.send('ok');
})
router.post('/del', (req, res) => {
    console.log('/user/del');
    res.send('ok');
})
module.exports = router;
```

# cookie

http协议有一个特性叫做无状态, 简单来说,客户端对服务端的每次请求,都是'初次见面',如果希望服务端可以识别客户端的请求，需要进行特殊处理

方式：

 客户端第一次请求服务端后，服务端进行响应时，给客户端下发‘小票’

- 下发的操作在响应头中，是一个set-cookie

- 客户端接收到响应信息后，浏览器会自动读取cookie并保存在本地,以后再次发送请求时，浏览器会自动将cookie信息添加在请求头中发送给服务端

```js
const express = require('express');
const bp = require('body-parser');
// 为了转换cookie数据的格式，可以使用第三方模块cookie-parser
const cp = require('cookie-parser');
let app = express();
// 配置bp模块,设置后可以使用核心模块querystring处理数据
app.use(bp.urlencoded({ extended: false }));

// 调用cp模块
app.use(cp());
// 静态资源托管
app.use(express.static('./public'));

// 1 设置登录接口进行登录信息检测
app.post('/login', (req, res) => {
  // 2 保存用户名和密码
  let { username: uname, password: pwd } = req.body;
  // 3 进行检测（admin和123456为正确）
  if (uname === 'admin' && pwd === '123456') {
    // - 为了完整正常的登录功能，需要在登录成功时，给用户下发cookie(小票)
    //   - 设置方式
    //      - 基本设置方式：res.cookie(名, 值); // 本次会话有效的cookie
    //      - 具体时间设置：res.cookie(名, 值, { expires: new Date(日期内容)});  expires 设置有效时间,默认是一次会话
      
    res.cookie('loginStatus', 'yes', {
      expires: new Date(Date.now() + 2 * 24 * 60 * 60 * 1000)
    })

    // 登录成功时，应当让用户查看首页index.html
    //  - 可以从服务端设置重定向操作(重定向指的是服务端设置的一种跳转)
    //    - 例如：客户端请求a.html，服务端认为a.html有问题，不希望用户看
    //       服务端设置重定向，让客户端查看b.html页面
      //res.redirect可以重定向
    res.redirect('http://localhost:8000/index.html');
  } else {
    res.send({
      status: 404,
      msg: '登录失败'
    });
  }

});

// 2 设置登录状态检测接口
app.get('/loginStatus', (req, res) => {
  // console.log(req.headers.cookie); // 传统的获取方式，数据形式很难操作

  // 使用cookie-parser中间件对cookie数据进行转换
  // req.cookies 用于获取cookie数据
  // console.log(req.cookies); // 对象形式的cookie数据

  // 2.1 检测cookie中的loginStatus的值是否为yes
  if (req.cookies.loginStatus === 'yes') {
    res.send({
      status: 200,
      msg: '已经登录过'
    });
  } else {
    res.send({
      status: 404,
      msg: '没有登录过'
    });
  } 
});

// 设置退出功能,删除cookie
app.get('/logout', (req, res) => {
    //express框架提供了一个删除方法。从服务器端删除:
  res.clearCookie('loginStatus');
  res.redirect('/login.html')
});

app.listen(8000, () => { console.log('监听8000成功..'); })
```

**理解cookie**

- cookie是将数据持久化（保存）存储到客户端（浏览器）的一种技术
- cookie是**键值对格式的字符串**
- 可以通过浏览器查看某个网站的cookie
- 如果浏览器保存了cookie，则向服务器发请求时，就会**自动带上这个cookie**。把cookie放在请求头中，发送给服务器。

# session

```js
// 普通cookie的数据时保存在客户端中，用户可以随意查看修改
//   - 如果用户手动设置loginStatus为yes，即便没有真的登录，也可以访问index.html

// 普通的cookie是不太安全的

// session的本质就是cookie的一种特殊用法而已
//   - 数据是保存在服务端，将保存数据的区域标记(称为sessionId)下发给客户端保存
//   - 下次请求发送到服务端时，这个标记也会自动发送到服务端，服务端再进行数据操作

// 小注意：并不是说session安全，就所有功能都必须使用session
//    - 例如点击关闭广告7天不在显示的这种记录功能使用普通的cookie即可

const express = require('express');
const bp = require('body-parser');

// session处理需要使用express-session的中间件进行处理操作
const es = require('express-session');

let app = express();
// 配置bp模块
app.use(bp.urlencoded({ extended: false }));

// 配置express-session模块
//  - 文档中指定了secret属性，用来设置加密字符串，但是不知道有什么效果
//  - 可以到文档中粘贴内容，或者随便书写一些内容即可
app.use(es({
  secret: 'itcast'
}));


// 静态资源托管
app.use(express.static('./public'));

// 1 设置登录接口进行登录信息检测
app.post('/login', (req, res) => {
  // 2 保存用户名和密码
  let { username: uname, password: pwd } = req.body;

  // 3 进行检测（admin和123456为正确）
  if (uname === 'admin' && pwd === '123456') {
    // 设置session中的数据
    //   - req.session是一个用于管理session信息的对象，可以进行读写操作
    req.session.loginStatus = 'yes';

    // 登录成功时，应当让用户查看首页index.html
    //  - 可以从服务端设置重定向操作(重定向指的是服务端设置的一种跳转)
    //    - 例如：客户端请求a.html，服务端认为a.html有问题，不希望用户看
    //       服务端设置重定向，让客户端查看b.html页面
    res.redirect('http://localhost:8000/index.html');
    /* res.send({
      status: 200,
      msg: '登录成功'
    }); */
  } else {
    res.send({
      status: 404,
      msg: '登录失败'
    });
  }

});

// 2 设置登录状态检测接口
app.get('/loginStatus', (req, res) => {

  // 2.1 检测session中的loginStatus的值是否为yes
  if (req.session.loginStatus === 'yes') {
    res.send({
      status: 200,
      msg: '已经登录过'
    });
  } else {
    res.send({
      status: 404,
      msg: '没有登录过'
    });
  }
});

// 3 设置退出接口
app.get('/logout', (req, res) => {
  // 清除session的方式
  req.session.destroy();
  // 设置重定向到登录页面即可
  res.redirect('/login.html');
});

app.listen(8000, () => { console.log('监听8000成功..'); })
```

## cookie、session原理

cookie原理：

- 从服务器端向客户端留下信息；每次访问时都带上

session原理：

- 服务器端会为每个用户（浏览器）各自保存一个session（文件）。当服务器保存session之后，会以cookie的形式告诉浏览器，你的session号是哪一个。它把session号返回给了浏览器，而把真实的数据保存在服务器。
- 下次再来访问服务器的时候，浏览器就会带着它自己的session号去访问，服务器根据session号就可以找到对应的session了。



## cookie和session的优缺点

cookie：优点是节省服务器空间，缺点不安全。不要保存敏感信息。

session：优点是安全，缺点需要服务器空间， 是一种最常见的解决方案。

#cors跨域

```js
// CORS 是标准中提供的一种跨域方式
//  (cross origin resource sharing) 跨域资源共享

// 本质：
//  - 服务端在响应头中设置一个信息
//    - Access-control-allow-origin: 域名

//  - 浏览器自动读取这个响应头，同时允许进行跨域ajax请求

const express = require('express');
let app = express();

app.post('/getCate', (req, res) => {
  // express中设置响应头使用res.header()即可
  // res.header('Access-control-allow-origin', '*'); // 允许所有非同源地址进行跨域请求（相当于公开的资源）
  // res.header('Access-control-allow-origin', 'http://localhost:4366'); // 指定某个网站地址设置

  // 如果希望设置多种情况，需要进行判断操作
  // 1 设置一段数据保存可以访问的地址
  let arr = ['http://localhost:5000', 'http://localhost:4366', 'http://localhost:8888'];

  // 2 获取本次请求的源的地址
  let origin = req.headers.origin;

  // 3 检测当前源是否处于可访问的列表中
  if (arr.includes(origin)) {
    // 4 给当前源设置响应头
    res.header('Access-control-allow-origin', origin);
  }

  res.send('ok');
});

app.listen(8000, () => {
  console.log('监听8000成功');

})
```

### jsonp vs cors 对比

jsonp：

- 不是ajax
- 只能使用`get方式`传参
- 兼容性好 

cors:

- 就是ajax
- 支持各种方式的请求(post,get....)
- 浏览器的支持不好

