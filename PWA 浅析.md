#  PWA 浅析

##### [Essay](https://dixinl.github.io/Essay/)

## PWA 对标于 Native App

名词释义：

- Native App（原生态应用）是现在最主流的mobile端应用，它的安全，性能，用户体验的确明显领先于其他互联网载体。

- PWA 是 Progressive Web App 的英文缩写， 翻译过来就是渐进式增强 WEB 应用， 是 Google 在 2016 年提出的概念，2017 年落地的 web 技术。目的就是在移动端利用提供的标准化框架，在网页应用中实现和原生应用相近的用户体验的渐进式网页应用。

1. 可靠——即时加载，即使在不确定的网络条件下也不会受到影响。
   当用户从主屏幕启动时，service work可以立即加载渐进式Web应用程序，完全不受网络环境的影响。service work就像一个客户端代理，它控制缓存以及如何响应资源请求逻辑，通过预缓存关键资源，可以消除对网络的依赖，确保为用户提供即时可靠的体验。
2. 快速——据统计，如果站点加载时间超过 3s，53% 的用户会放弃等待。页面展现之后，用户期望有平滑的体验，过渡动画和快速响应。
3. 沉浸式体验—— 感觉就像设备上的原生应用程序，具有沉浸式的用户体验。
   渐进式Web应用程序可以安装并在用户的主屏幕上，无需从应用程序商店下载安装。他们提供了一个沉浸式的全屏幕体验，甚至可以重新与用户接触的Web推送通知。

PWA 并不单指某一项技术，它是一种概念，利用 Web 技术使网页实现类似于 App 一样的功能，提升其安全性，性能，流畅性，用户体验等各方面指标，最后使用户就像在用 App 一样的感觉。

### PWA中包含的核心功能及特性如下：

1. **Web App Manifest**
2. **Service Worker**
3. **Cache API 缓存**
4. **Push&Notification 推送与通知**
5. **Background Sync 后台同步**
6. **响应式设计**

## PWA 如何让 Web 页面看起来像一个 App

- 性能差异：PWA 使用 **App Shell** 架构，将网页缓存并在下次使用时对必要信息更新
- 离线使用：**Service Worker + HTTPS +Cache Api + indexedDB** 等一系列web技术实现离线加载和缓存
- 数据更新：**Background Sync** 后台同步技术
- 服务端推送：**Push&Notification** 实现推送与通知
- 桌面图标：通过 **manifest.json** 文件配置，使得可以直接添加到手机的桌面上。

## 前景

PWA 未来的发展不做讨论，不过在国内类似功能的产物——小程序的发展倒是很迅速，但这就不是技术层面的事了。它是市场、商业、社会多方面影响所诞生的结果。

## 参考

百度的 [LAVAS](https://lavas.baidu.com/pwa/README)，基于 Vue.js 的 PWA 解决方案

## Manifest.json 文件

### 使用

> ```html
> <link rel="manifest" href="/manifest.json">
> ```

### 范例

> ```json
> {
>   "name": "HackerWeb",	//为应用程序提供一个人类可读的名称，例如在其他应用程序的列表中或作为图标的标签显示给用户。
>   "short_name": "HackerWeb",	//为应用程序提供简短易读的名称。 这是为了在没有足够空间显示 Web应用程序的全名时使用。
>   "lang": "zh-CN",	//指定 name 和 short_name 成员中的值的主要语言。 该值是包含单个语言标记的字符串。
>   "start_url": ".",	//指定用户从设备启动应用程序时加载的 URL。 如果以相对 URL 的形式给出，则基本 URL 将是 manifest 的 URL。
>   "display": "standalone",	//定义开发人员对 Web 应用程序的首选显示模式。
>   "background_color": "#fff",	//为 web 应用程序预定义的背景颜色。此值在应用程序样式表中可以再次声明。它主要用于在样式表加载之前，当加载 manifest，浏览器可以用来绘制 web 应用程序的背景颜色。在启动 web 应用程序和加载应用程序的内容之间创建了一个平滑的过渡。	--注意：background_color 只是在 web 应用程序加载时提高用户体验，而当 web 应用程序的样式表可用时，不能替代作为背景颜色使用。
>   "description": "A simply readable Hacker News app.",	//提供有关Web应用程序的一般描述。
>   "icons": [{
>     "src": "images/touch/homescreen48.png",	//图像文件的路径。 如果 src 是一个相对 URL，则基本 URL 将是 manifest 的 URL。
>     "sizes": "48x48",	//包含空格分隔的图像尺寸的字符串。
>     "type": "image/png"	//提示图像的媒体类型。此字段的目的是允许用户代理快速忽略不支持的媒体类型的图像。
>   }, {
>     "src": "images/touch/homescreen72.png",
>     "sizes": "72x72",
>     "type": "image/png"
>   }, {
>     "src": "images/touch/homescreen96.png",
>     "sizes": "96x96",
>     "type": "image/png"
>   }, {
>     "src": "images/touch/homescreen144.png",
>     "sizes": "144x144",
>     "type": "image/png"
>   }, {
>     "src": "images/touch/homescreen168.png",
>     "sizes": "168x168",
>     "type": "image/png"
>   }, {
>     "src": "images/touch/homescreen192.png",
>     "sizes": "192x192",
>     "type": "image/png"
>   }],	//指定可在各种环境中用作应用程序图标的图像对象数组。 例如，它们可以用来在其他应用程序列表中表示 Web 应用程序，或者将 Web 应用程序与 OS 的任务切换器和 / 或系统偏好集成在一起。
>   "related_applications": [{
>     "platform": "web"	//可以找到应用程序的平台。
>   }, {
>     "platform": "play",
>     "id": "cheeaun.hackerweb",	//用于表示指定平台上的应用程序的 ID。
>     "url": "https://play.google.com/store/apps/details?id=cheeaun.hackerweb"	//可以找到应用程序的 URL。
>   }]	//指定一个“应用程序对象”数组，代表可由底层平台安装或可访问的本机应用程序 - 例如可通过 Google Play Store 获取的原生 Android 应用程序。 这样的应用程序旨在替代提供类似或等同功能的 Web 应用程序 - 就像 Web 应用程序的本机应用程序版本一样。
> }
> ```

### 其他

- ###### prefer_related_applications

  - 指定一个布尔值，提示用户代理向用户指示指定的相关应用程序（请参见下文）可用，并建议通过Web应用程序。 只有当相关的本地应用程序确实提供了某些Web应用程序无法做到的事情时，才应该使用它。

    > ```json
    > "prefer_related_applications": false
    > ```

  - Note: 如果省略,默认设置为 false。

- ###### dir

  - 指定名称、短名称和描述成员的主文本方向。与lang一起配置，可以帮助正确显示右到左文本。

    > ```json
    > "dir": "rtl",
    > "lang": "ar",
    > "short_name": "أنا من التطبيق!"
    > ```

  - 可选值:
    - ltr: 由左到右
    - rtl: 由右到左
    - auto: 由浏览器自动判断

  * 注意：当省略时，默认为auto

- ###### display

  - 有效值

    | 显示模式   | 描述                                                         | 后备显示模式 |
    | ---------- | :----------------------------------------------------------- | ------------ |
    | fullscreen | 全屏显示, 所有可用的显示区域都被使用, 并且不显示状态栏 [chrome](https://developer.mozilla.org/en-US/docs/Glossary/chrome)。 | standalone   |
    | standalone | 让这个应用看起来像一个独立的应用程序，包括具有不同的窗口，在应用程序启动器中拥有自己的图标等。这个模式中，用户代理将移除用于控制导航的 UI 元素，但是可以包括其他 UI 元素，例如状态栏。 | minimal-ui   |
    | minimal-ui | 该应用程序将看起来像一个独立的应用程序，但会有浏览器地址栏。 样式因浏览器而异。 | browser      |
    | browser    | 该应用程序在传统的浏览器标签或新窗口中打开，具体实现取决于浏览器和平台。 这是默认的设置。 | (None)       |

    - 注：
      	1. 在一个浏览器中，Chrome 指除了网页本身之外任何可视的部分 UI（如：工具栏、菜单栏、标签）。这不应与 Google Chrome 浏览器混淆。
       	2. 可以使用显示模式媒体功能，根据[显示模式](https://developer.mozilla.org/docs/Web/CSS/@media/display-mode)选择性地将CSS应用到您的应用程序。 这可用于在从URL启动网站和从桌面图标启动网站之间提供一致的用户体验。

    > ```json
    > "display": "standalone"
    > ```

- ###### orientation

  - 定义所有Web应用程序顶级的默认方向 [browsing contexts](https://developer.mozilla.org/en-US/docs/Glossary/Browsing_context).

  - 方向可以是以下值之一：

    - any
    - natural
    - landscape
    - landscape-primary
    - landscape-secondary
    - portrait
    - portrait-primary
    - portrait-secondary

    > ```json
    > "orientation": "portrait-primary"
    > ```

- ###### scope

  - 定义此 Web 应用程序的应用程序上下文的导航范围。 这基本上限制了 manifest 可以查看的网页。 如果用户在范围之外浏览应用程序，则返回到正常的网页。

  - 如果 scope 是相对 URL，则基本 URL 将是 manifest 的 URL。

    > ```json
    > "scope": "/myapp/"
    > ```

- ###### theme_color

  - 定义应用程序的默认主题颜色。 这有时会影响操作系统显示应用程序的方式（例如，在 Android 的任务切换器上，主题颜色包围应用程序）。

    > ```json
    > "theme_color": "aliceblue"
    > ```

### 初始屏幕

在Chrome 47及更高版本中，从主屏幕启动的Web应用程序将显示启动画面。 这个启动画面是使用Web应用程序清单中的属性自动生成的，具体来说就是：`name`，`background_color`以及`icons` 中距设备最近128dpi的图标。

### Mime类型

Manifests 应使用 application/manifest+json 的 MIME 类型. 但是，这不是必须的。