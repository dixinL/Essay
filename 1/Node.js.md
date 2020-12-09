# Node.js

**没废话，写到哪算哪，有时间再整理吧**

## supervisor

- 监视文件的改动，并自动重启node

> $ npm install -g supervisor
>
> supervisor script.js

## server

- 通过调用 http 模块，创建一个服务并监听 3000 端口

> ```javascript
> //app.js
> 
> var http = require('http'); 
> http.createServer(function(req, res) { 
>  	res.writeHead(200, {'Content-Type': 'text/html'}); 
>  	res.write('<h1>Node.js</h1>'); 
>  	res.end('<p>Hello World</p>'); 
> }).listen(3000); 
> console.log("HTTP server is listening at port 3000.");
> ```
>
> 

## readFile

- 异步事件读取文件内容

> ```javascript
> //readfile.js
> 
> var fs = require('fs'); 
> fs.readFile('file.txt', 'utf-8', function(err, data) { 
>     if (err) { 
>  		console.error(err); 
>  	} else { 
>  		console.log(data); 
>  	} 
> }); 
> console.log('end.');
> ```

- 同步事件

> ```javascript
> //readfilesync.js
> 
> var fs = require('fs'); 
> var data = fs.readFileSync('file.txt', 'utf-8'); 
> console.log(data); 
> console.log('end.');
> ```

## events

- event.on 注册事件的监听器，当 event.emit 发送某一事件后，会调用该监听器事件。

> ```javascript
> //event.js
> 
> var EventEmitter = require('events').EventEmitter; 
> var event = new EventEmitter(); 
> event.on('some_event', function() { 
>  	console.log('some_event occured.'); 
> }); 
> setTimeout(function() { 
>  	event.emit('some_event'); 
> }, 1000);
> ```

## module

- 模块和包可以理解为一个概念，只不过包可以发布、更新、依赖管理和版本控制，通过 require 引入。
- require 不会重复引入模块

> ```javascript
> //module.js
> 
> var name; 
> exports.setName = function(thyName) { 
>  	name = thyName; 
> }; 
> exports.sayHello = function() { 
>  	console.log('Hello ' + name); 
> };
> 
> //getmodule.js
> 
> var myModule = require('./module'); 
> myModule.setName('BYVoid'); 
> myModule.sayHello();
> ```

- 覆盖 exports，这看起来和我们平常在vue.js中使用的 node_modules 的方法类似了，如果使用 bable 插件，import 引入那就更好了！

> ```javascript
> //hello.js
> 
> function Hello() { 
>  	var name; 
>  	this.setName = function(thyName) { 
>  		name = thyName; 
> 	}; 
> 	this.sayHello = function() { 
>  		console.log('Hello ' + name); 
>  	}; 
> }; 
> module.exports = Hello;
> 
> //gethello.js
> 
> var Hello = require('./hello'); 
> hello = new Hello(); 
> hello.setName('BYVoid'); 
> hello.sayHello();
> ```

此处使用 module.exports = Hello 代替了 exports.Hello = Hello，外部引用时直接引用 Hello 这个对象。

exports 本身是一个对象，它用来声明接口，通过它为模块内部建立了一个接口。

- 不可以通过对 exports 直接赋值代替对 module.exports 赋值。 exports 实际上只是一个和 module.exports 指向同一个对象的变量， 它本身会在模块执行结束后释放，但 module 不会，因此只能通过指定 module.exports 来改变访问接口。