## 遇到的问题

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
