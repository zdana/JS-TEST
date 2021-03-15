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
        