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