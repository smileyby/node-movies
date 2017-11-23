## 遇到的问题（所有问题都是在windows下遇到的）

* window下修改node启动服务的端口号
> 参考文章：[https://stackoverflow.com/questions/9249830/how-can-i-set-node-env-production-on-windows](https://stackoverflow.com/questions/9249830/how-can-i-set-node-env-production-on-windows) 
> 且在`app.js`中端口变量代码修改为`var port = process.env.NODE_ENV || 3000`
> 然后在PowerShell中通过命令`$env:NODE_ENV="1234"` 和 `node app.js` 再次启动服务，现在就已经运行在1234端口了。

* 在调用`express.bodyParser()`报错如下：

```
Error: Most middleware (like bodyParser) is no longer bundled with Express and m  
ust be installed separately. Please see https://github.com/senchalabs/connect#mi  
ddleware.  
...
```
> 根据报错信息可知，bodyparser已经不再和express绑定在一起了，需要单独安装运行`npm install body-parser`即可完成安装
> 
> 在项目用需要单独引用
```
var bodyParser = require('body-parser');
app.use(bodyParser());
```
> 然后发现依然报错：

```
body-parser deprecated bodyParser: use individual json/urlencoded middlewares ex  
press1.js:4:9  
body-parser deprecated undefined extended: provide extended option node_modules\  
body-parser\index.js:75:29  
express deprecated res.send(status): Use res.status(status).end() instead expres  
s1.js:6:6  
```
> 发现已经过期了，可以使用如下代码代替：

```
// parse application/x-www-form-urlencoded  
app.use(bodyParser.urlencoded({ extended: false }))  
  
// parse application/json  
app.use(bodyParser.json()) 
```
在后面使用的过程中发现，直接使用`app.use(bodyParser())`就可以了。引用上面的两句会出现问题

* 访问页面报错`Error: Failed to lookup view "pages/list" in views directory "./views/pages"`

> 在访问list页的时候报错，经过各种百度 google stackoverflow，最终发现，原来的文件名字拼写错误。

* 发现后台录入页的样式对不上

> 检查后发现，`form`拼写成`from` 导致样式没引用到，这种问题真的好难看到

* 关于mongoose 模式 模型 文档

* mongoose 报错：

```
(node:1556) DeprecationWarning: `open()` is deprecated in mongoose >= 4.11.0, us
e `openUri()` instead, or set the `useMongoClient` option if using `connect()` o
r `createConnection()`. See http://mongoosejs.com/docs/connections.html#use-mong
o-client
```

> 解决办法：[http://blog.csdn.net/yingzizizizizizzz/article/details/74942107](http://blog.csdn.net/yingzizizizizizzz/article/details/74942107)

* 后台录入页报错：`TypeError: Cannot read property 'movie' of undefined`

> 检查发现是`body-parser`解析出错，报错前后代码对比如下是如下：

```
// 报错的
var bodyParser = require('body-parser')
app.use(bodyParser.urlencoded({ extended: false }))  
app.use(bodyParser.json())  
```

```
// 不报错的
var bodyParser = require('body-parser')
app.use(bodyParser());
```

至于为什么这样写就报错了，暂时还不清楚

* mongoose清除本地测试数据

```
mongoose.Promise = global.Promise;
mongoose.connect('mongodb://localhost/node-movies', {useMongoClient: true})
```

链接数据库的部分我是这样写的，可以看到我链接的本地数据库名字叫`node-movies`

找到本地安装的mongoDB目录进入`bin`目录下找到`mongo.exe`双击运行

```
show dbs // 查看本地所有的数据库
use node-movies // 进入到'node-movies'数据库
db.movies.find({}) // 查找当前数据库下存储的所有movies数据
db.movies.find({}).count() // 统计movies一共有多少条
db.movies.remove({}) // 移除所有movies存储记录
```

在`mongo.exe`运行完以上命令即可清除本地的测试数据了

* bower 新建配置文件`.bowerrc`

```
{
  "directory": "public/libs"
}
```

> 该文件用来配置bower下载的文件目录,正常情况下默认下载到`bower_components`目录下，如果存在`.bowerrc`文件则下载到该文件所配置的下载目录下

* 使用jade模板一定要注意缩进问题