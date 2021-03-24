## 本项目主要是针对练习js、html一些特性、方式

### 1. script标签
+ 建议放在body闭合标签前
+ JavaScript代码是载入后立刻执行的；执行时会阻塞页面后续的内容（如页面的渲染、图片资源的加载）
+ 加载body标签后能确保在脚本执行前已完成对dom树的渲染
+ 提高性能的方法
    序号|内容
    --|:--:
    1|将script标签放在body闭合标签之前|
    2|尽可能合并脚本，即script中标签越少，加载越快，响应越迅速|
    3|采用无阻塞下载 JavaScript 脚本的方法|
+ 区分为两种：
    + 放在head标签中：
        + 需调用/事件触发执行的脚本
    + 放在body标签中：
        + 页面被加载时立刻执行的脚本
+ 最优解的一边解析页面一边下载JS实现方式：
    ```
    <script type="text/javascript" src="path/to/script1.js" async></script>  
    <script type="text/javascript" src="path/to/script2.js" async></script>  
    ```
    + 让浏览器做到一边下载JS(还是只能同时下载两个JS)，一边解析HTML;
    + async属性能保证script会异步执行，只要下载完就执行;
    + defer属性就能保证script有序执行，script1.js先执行，script2.js后执行
+ **script放置位置的原则“页面效果实现类的js应该放在body之前，动作，交互，事件驱动的js都可以放在body之后**
### 2. link标签  
+ 推荐放在head标签中
    + 原因：浏览器代码解析是从上到下的；
    + 如果把css放在底部，当网速慢时，html代码加载完成后而css没加载完成，会导致页面没有样式而难以阅读，因此先加载css而让页面正常显示

### 3. 浏览器返回状态码含义
类别|内容|描述|
--|:--|:--
1xx:信息|100|服务器接收部分请求|
1xx:信息|101|服务器转换协议|
2xx:成功|200|请求成功，获取get/post请求文档|
2xx:成功|201|请求和新资源被创建|
2xx:成功|202|请求被接受，处理未完成|
3xx:重定向|301|请求页面已转移到新的url|
3xx:重定向|302|请求页面已临时转移到新的url|
4xx:客户端错误|400|服务器未能理解请求|
4xx:客户端错误|401|被请求的页面需要用户名和密码|
4xx:客户端错误|403|拒绝访问该页面|
4xx:客户端错误|404|无法找到请求的页面|
4xx:客户端错误|405|请求方法不被允许|
5xx:服务器错误|500|请求未完成，服务器遇到未知情况|
5xx:服务器错误|501|请求未完成，服务器不支持所请求的功能|
5xx:服务器错误|502|请求未完成，服务器从上游服务器收到一个无效响应|
5xx:服务器错误|503|请求未完成，服务器临时过载/当机|
5xx:服务器错误|504|网关超时|

### 4. IE盒子模型与W3C盒子模型(测试详见兼容1)

IE=height+margin
W3C=height+padding+margin

### 5.Promises
+ 用于解决回调金字塔(回调圣诞树/回调地狱)
+ Promises写法本质：将异步写法改成同步写法，原因：
    + js是单线程运行，类似于流水线，单条单条执行；
        + 所有任务都要排队；
        + 若单条任务执行时间过长，会导致进程阻塞
    + 分为三种状态：pending/rejected/resolved
        + resolved：将状态从未完成改成成功，在异步操作成功时调用，并将异步操作的结果作为参数传递出去
        + rejected：将状态从未成功改为失败，在异步操作失败时调用，并将异步操作报出的错误作为参数传递出去
+ 传入Promises构造函数的函数参数会优先执行第一行代码，按顺序执行代码；
+ .them会等到promise对象实例有结果后再执行其中的代码
+ resolved/rejected后的代码依旧会执行，resolved/rejected只是表示传参，将pending状态转为rejected/resolved；
  若不写rejected/resolved则之后的then不执行<==原因：此时一直处于pending状态
+ 实例详见promise

### 6.常见浏览器兼容
序号|问题|解决方案|备注
--|:--|:--|:--
01|不同浏览器的padding和margin|*{padding:0;margin:0}||
02|在Firefox中可用const定义常量，IE中只能用var定义常量|统一用var定义常量|
03|事件绑定|IE：attachEvent();其他浏览器：addeventListener|
04|IE与其他浏览器不同|IE：ActiveXObject;其他浏览器：XMLHttpRequest|
05|IE中event有srcElement属性，没有target属性；其他有target属性，没有sreElement属性|srcObj=event.srcElement?event.srcElement:targetevent.|
06|透明度IE与其他浏览器不同|IE：filter:progid:DXImageTransform.Microsoft.Alpha(style=0,opacity=60)。FF：opacity:0.6|
07|IE与其他浏览器盒子模型不同|.box{width:100px;border:2px}div{margin:30px !importent;margin:28px}|IE的盒子模型包括border;!importent属性只有ie无法识别；
08|ul和ol的列表样式及缩紧（ie、firefox、其他浏览器）|list-style:node;margin:0;padding:0|
09|水平居中|IE：父元素text-align:center;其他浏览器：margin:0 auto|
10|margin加倍(ie6会出现)|div添加属性，display:inline|div在flot情况下设置margin会加倍|
11|高度自适应|内部元素上下各添加一个css属性为{height:0;overflow:hidden}的div|内层高度发生变化时，外层高度不能进行自动调节，特别是内层的margin和padding改变时|
12|内容超过长度出现省略号|{width:200px;white-space:nowrap;text-overflow:ellipsis;-o-text-overflow:ellipsis;overflow:hidden;}|目前仅适用于ie，safari，chrome
13|a标签点击过后hover样式不再出现|a:link {}a:visited {}a:hover {}a:active {}|改变这四个的顺序|

### 7. h5中唤醒手机拨打电话
+ 在heade标签中添加
```
    <meta name="format-detection" content="telephone=yes"/>
```
+ 在js中加入：window.location.href='tel:400-000-000'
+ 或在html的a标签中写入href='tel:'400-000-000'

### 8. h5中唤醒手机发送短信
+ 在js中加入：window.location.href='sms:10086?body=短信内容'
+ 或在html的a标签中写入href='sms:10086?body=短信内容'

### 9. 进制转换
+ x.toString(2) :表示将x转为2进制
+ 将String类型转为Int类型有两种方法：
    + Number(x):推荐使用改种方法
    + parsenInt(x)
+ x.toString() :表示将x有int型转换为string型

### 10. 使用git
+ 在git上创建项目后如果此时项目唯恐，则必须先commit，创建新分支，再进行推送
+ git init
+ git add README.md
+ git commit -m "first commit"
+ git branch -M main
+ git remote add origin git@github.com:zdana/vue-exercises.git
+ git push -u origin main

### 11. canvas没有代码提示
+ 在script使用canvas的代码前加上/** @type {HTMLCanvasElement} */

### 12. svg和canvas的区别
+ svg是利用dom的方式进行编程；在放大的场景下获得更高的清晰度；适合做图标
+ canvas是利用jsapi进行编程；相比较而言有更高的绘图效率

### 13. zrender
+ 是二维绘图引擎，提供canvas、svg、vml等多种渲染方式；
+ 是Echarts的渲染引擎；