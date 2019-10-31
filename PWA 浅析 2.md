# PWA 浅析 2

##### [Essay](https://dixinl.github.io/Essay/)

## Service Work

**本文重点讲解 PWA 中关键部分—— Service Work。**

Service Worker 简单地理解为一个独立于前端项目，在后台运行的进程。因此，它不会阻塞浏览器脚本的运行，同时也无法直接访问浏览器相关的API（例如：DOM、localStorage等）。此外，即使在离开 Web App，甚至是关闭浏览器后，service work 仍然可以运行。来处理着缓存、推送、通知与同步等工作。所以，要学习PWA，绕不开的就是Service Worker。由于Service Work的权限很高，所有的代码都需要是安全的，所以 Service Worker 只能运行在 **HTTPS** 域下，也可以运行在 **localhost（127.0.0.1）**域下。

![xapsgw6e7j](images/xapsgw6e7j.jpeg)

客服端所有发起的请求都会被 fetch 这个监听器所监听。

## Service Worker作用

1.网络代理，转发请求，伪造响应 
2.离线缓存 
3.消息推送 
4.后台消息传递

## SW 注意点

- 它是 JavaScript Worker，所以它不能直接操作 DOM。但是 Service Worker 可以通过 postMessage 与页面之间通信，把消息通知给页面，如果需要的话，让页面自己去操作 DOM。
- Service Worker 是一个可编程的网络代理，允许开发者控制页面上处理的网络请求。
- 在不被使用的时候，它会自己终止，而当它再次被用到的时候，会被重新激活，所以你不能依赖于 Service Worker的 onfecth 和 onmessage 的处理函数中的全局状态。如果你想要保存一些持久化的信息，你可以在Service Worker 里使用 IndexedDB API。
- Service Worker 大量使用 promise。

## SW 生命周期

Service Worker 拥有一个完全独立于 Web 页面的生命周期。

Service Worker 要经历以下过程： 

1. 安装 
2. 激活，激活成功之后，打开 Chrome://inspect/#service-workers 可以查看到当前运行的 Service Worker 
3. 监听 fetch 和 message 事件，下面两种事件会进行简要描述 
4. 销毁，是否销毁由浏览器决定，如果一个 Service Worker 长期不使用或者机器内存有限，则可能会销毁这个Worker

要让一个 Service Worker 在网站上生效，需要先在网页中注册它。注册一个 Service Worker 之后，浏览器会在后台默默启动一个 Service Worker 的安装过程。在安装过程中，浏览器会加载并缓存一些静态资源。如果所有的文件被缓存成功，Service Worker 就安装成功了。如果有任何文件加载或缓存失败，那么安装过程就会失败，Service Worker 就不能被激。如果发生这样的问题，它会在下次再尝试安装。

当安装完成后，Service Worker 的下一步是激活，如果想要升级一下 Service Worker 的版本，也是在这个阶段。

在激活之后，Service Worker 将接管所有在自己管辖域范围内的页面，但是如果一个页面是刚刚注册了 Service Worker，那么它这一次不会被接管，到下一次加载页面的时候，Service Worker 才会生效。 
当 Service Worker 接管了页面之后，它可能有两种状态：要么被终止以节省内存，要么会处理 fetch 和 message 事件，这两个事件分别产生于一个网络请求出现或者页面上发送了一个消息。 

![20170215223617715](images/20170215223617715.png)

当我们注册了 Service Worker 后，它会经历生命周期的各个阶段，同时会触发相应的事件。整个生命周期包括了：installing --> installed --> activating --> activated --> redundant。当 Service Worker 安装（installed）完毕后，会触发install事件；而激活（activated）后，则会触发 activate 事件。

## SW 与页面
在页面发起 http 请求时，Service Worker 可以通过 fetch 事件拦截请求，并且给出自己的响应。在前端开发中通过 fetch 事件拦截请求是很基本的技术。 
W3C 提供了一个新的 fetch api，用于取代 XMLHttpRequest，与 XMLHttpRequest 最大不同有两点： 

- fetch() 方法返回的是 Promise 对象，通过 then 方法进行连续调用，减少嵌套。
- 提供了 Request、Response 对象，如果做过后端开发，对 Request、Response 应该比较熟悉。前端要发起请求可以通过 url 发起，也可以使用 Request 对象发起，而且 Request 可以复用。但是 Response 用在哪里呢？在 Service Worker 出现之前，前端不会自己给自己发消息，但是有了 Service Worker，就可以在拦截请求之后根据需要发回自己的响应，对页面而言，这个普通的请求结果并没有区别。



页面和 Service Worker 之间可以通过 posetMessage() 方法发送消息，发送的消息可以通过 message 事件接收到，这是一个双向的过程：页面可以发消息给 Service Worker，Service Worker 也可以发送消息给页面，由于这个特性，可以将 Service Worker 作为中间纽带，使得一个域名或者子域名下的多个页面可以自由通信。

## SW 缓存文件
通过 Service Worker 缓过的文件，只要访问过一次，下一次就可实现离线访问了。

准备 index.js，用于注册 Service Worker

> ```javascript
> if ('serviceWorker' in navigator) {
>   navigator.serviceWorker.register('/sw.js').then(function(registration) {
>     // Registration was successful
>     console.log('ServiceWorker registration successful with scope: ',    registration.scope);
>   }).catch(function(err) {
>     // registration failed :(
>     console.log('ServiceWorker registration failed: ', err);
>   });
> }
> ```

在上述代码中，注册了 service-worker.js 作为当前路径下的 Service Worker。 

在一个新的 Service Worker 被注册以后，首先会触发 install 事件。可以通过监听 install 事件做一些初始化工作。 

因为我们是要缓存离线文件，所以可以在 install 事件中开始缓存并决定哪些文件需要被缓存。
首先定义了需要缓存的文件数组 cacheFile，然后在 install 事件中，缓存这些文件。

> ```javascript
> // The files we want to cache
> var urlsToCache = [
>   '/',
>   '/styles/main.css',
>   '/script/main.js'
> ];
> 
> // Set the callback for the install step
> self.addEventListener('install', function(event) {
>     // Perform install steps
> });
> ```

当 Service Worker 被安装成功并且用户浏览了另一个页面或者刷新了当前的页面，Service Worker 将开始接收到 fetch 事件。下面是一个例子：

> ```js
> self.addEventListener('fetch', function(event) {
>   event.respondWith(
>     caches.match(event.request)
>       .then(function(response) {
>         // Cache hit - return response
>         if (response) {
>           return response;
>         }
> 
>         return fetch(event.request);
>       }
>     )
>   );
> });
> ```

上面的代码里我们定义了 fetch 事件，在 event.respondWith 里，我们传入了一个由 caches.match 产生的 promise.caches.match 查找 request 中被 Service Worker 缓存命中的 response。

如果我们有一个命中的response，我们返回被缓存的值，否则我们返回一个实时从网络请求fetch的结果。这是一个非常简单的例子，使用所有在install步骤下被缓存的资源。

如果我们想要增量地缓存新的请求，我们可以通过处理fetch请求的response并且添加它们到缓存中来实现，例如：

> ```js
> self.addEventListener('fetch', function(event) {
>   event.respondWith(
>     caches.match(event.request)
>       .then(function(response) {
>         // Cache hit - return response
>         if (response) {
>           return response;
>         }
> 
>         // IMPORTANT: Clone the request. A request is a stream and
>         // can only be consumed once. Since we are consuming this
>         // once by cache and once by the browser for fetch, we need
>         // to clone the response
>         var fetchRequest = event.request.clone();
> 
>         return fetch(fetchRequest).then(
>           function(response) {
>             // Check if we received a valid response
>             if(!response || response.status !== 200 || response.type !== 'basic') {
>               return response;
>             }
> 
>             // IMPORTANT: Clone the response. A response is a stream
>             // and because we want the browser to consume the response
>             // as well as the cache consuming the response, we need
>             // to clone it so we have 2 stream.
>             var responseToCache = response.clone();
> 
>             caches.open(CACHE_NAME)
>               .then(function(cache) {
>                 cache.put(event.request, responseToCache);
>               });
> 
>             return response;
>           }
>         );
>       })
>     );
> });
> ```

代码里我们所做事情包括：

1. 添加一个 callback 到 fetch 请求的 .then 方法中
2. 一旦我们获得了一个 response，我们进行如下的检查：
   1. 确保 response 是有效的
   2. 检查 response 的状态是否是 200
   3. 保证 response 的类型是 basic（标准值，同源响应, 带有所有的头部信息除了 “Set-Cookie”  和  “Set-Cookie2″），这表示请求本身是同源的，非同源（即跨域）的请求不能被缓存。
3. 如果我们通过了检查，clone() 这个请求。这么做的原因是如果 response 是一个 Stream（该规范提供了用于创建，组合和使用有效映射到低级 I / O 原语的数据流的 API），那么它的body只能被读取一次，所以我们得将它克隆出来，一份发给浏览器，一份发给缓存。

## 如何更新一个Service Worker

你的 Service Worker 总有需要更新的那一天。当那一天到来的时候，你需要按照如下步骤来更新：

1. 更新你的 Service Worker 的 JavaScript 文件
   1. 当用户浏览你的网站，浏览器尝试在后台下载 Service Worker 的脚本文件。只要服务器上的文件和本地文件有一个字节不同，它们就被判定为需要更新。
2. 更新后的 Service Worker 将开始运作，install event 被重新触发。
3. 在这个时间节点上，当前页面生效的依然是老版本的 Service Worker，新的 Servicer Worker 将进入 "waiting" 状态。
4. 当前页面被关闭之后，老的 Service Worker 进程被杀死，新的 Servicer Worker 正式生效。
5. 一旦新的 Service Worker 生效，它的 activate 事件被触发。

代码更新后，通常需要在 activate 的 callback 中执行一个管理 cache 的操作。因为你会需要清除掉之前旧的数据。我们在 activate 而不是 install 的时候执行这个操作是因为如果我们在 install 的时候立马执行它，那么依然在运行的旧版本的数据就坏了。

之前我们只使用了一个缓存，叫做 "my-site-cache-v1"，其实我们也可以使用多个缓存的，例如一个给页面使用，一个给 blog 的内容提交使用。这意味着，在 install 步骤里，我们可以创建两个缓存，"pages-cache-v1" 和 "blog-posts-cache-v1"，在 activite 步骤里，我们可以删除旧的 "my-site-cache-v1"。

下面的代码能够循环所有的缓存，删除掉所有不在白名单中的缓存。

> ```JS
> self.addEventListener('activate', function(event) {
>   var cacheWhitelist = ['pages-cache-v1', 'blog-posts-cache-v1'];
>   event.waitUntil(
>     caches.keys().then(function(cacheNames) {
>       return Promise.all(
>         cacheNames.map(function(cacheName) {
>           if (cacheWhitelist.indexOf(cacheName) === -1) {
>             return caches.delete(cacheName);
>           }
>         })
>       );
>     })
>   );
> });
> ```