# 主要功能（统计指标）

## 访问速度

在 ARMS 中，访问速度是指页面的首次渲染时间。

在性能测速统计中，所有数据都是根据 W3C 规范中定义的 [Navigation Timing API](https://www.w3.org/TR/navigation-timing/) 计算出来的。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/152286/155496247543777_zh-CN.png)

**字段含义**

|上报字段|描述|计算方式|备注|
|:--:|::|:--:|::|
|dns|DNS 解析耗时|domainLookupEnd - domainLookupStart| |
|tcp|TCP 连接耗时|connectEnd - connectStart| |
|ssl|SSL 安全连接耗时|connectEnd - secureConnectionStart|只在 HTTPS 下有效|
|ttfb|Time to First Byte（TTFB），网络请求耗时|responseStart - requestStart|TTFB 有多种计算方式，ARMS 以 [Google Development 定义](https://developers.google.com/web/tools/chrome-devtools/network-performance/reference#timing)为准|
|trans|数据传输耗时|responseEnd - responseStart| |
|dom|DOM 解析耗时|domInteractive - responseEnd| |
|res|资源加载耗时|loadEventStart - domContentLoadedEventEnd|表示页面中的同步加载资源|

|上报字段|描述|计算方式|备注|
|----|--|----|--|
|firstbyte|首包时间|responseStart - domainLookupStart| |
|fpt|First Paint Time, 首次渲染时间 / 白屏时间|responseEnd - fetchStart|从请求开始到浏览器开始解析第一批 HTML 文档字节的时间差|
|tti|Time to Interact，首次可交互时间|domInteractive - fetchStart|浏览器完成所有 HTML 解析并且完成 DOM 构建，此时浏览器开始加载资源|
|ready|HTML 加载完成时间， 即 DOM Ready 时间|domContentLoadEventEnd - fetchStart|如果页面有同步执行的 JS，则同步 JS 执行时间 = ready - tti|
|load|页面完全加载时间|loadEventStart - fetchStart|load = 首次渲染时间 + DOM 解析耗时 + 同步 JS 执行 + 资源加载耗时|

## 满意度

性能指数 [APDEX](http://www.apdex.org/)（全称 Application Performance Index）是一个国际通用的应用性能计算标准。该标准将用户对应用的体感定义为三个等级：

-   满意（0 ~ T）
-   可容忍（T ~ 4T）
-   不满意（大于 4T）

计算公式为：

```
Apdex = (满意数 + 可容忍数 / 2) / 总样本量
```

<img src="http://apdex.org/images/overview_figure1_performancezones_256_111.gif">


ARMS 取页面首次渲染时间（First Paint Time）作为计算指标，默认定义 T 为 2 秒。

## JS 稳定性

JS 稳定性在 ARMS 中是指页面的 JS 错误率。

在一个 PV 周期内，如果发生过错误（JS Error），则此 PV 周期为错误样本。

```
错误率 = 错误样本量 / 总样本量
```

页面异常除了自动上报的 JS Error 外，也包括手动调用 [API 使用指南](cn.zh-CN/前端监控/高级选项/API 使用指南.md#)上报的错误。




## API 成功率/耗时

```
API成功率 = 接口调用成功的样本量 / 总样本量
```

统计 API 成功率的样本除了自动上报的 AJAX 请求，还包括手动调用 [API 使用指南](cn.zh-CN/前端监控/高级选项/API 使用指南.md#)上报的数据。

## PV/UV
<img src="https://static.dingtalk.com/media/lALPDgQ9rTlgkAXNAVnNA1Y_854_345.png_620x10000q90g.jpg?auth_bizType=IM&auth_bizEntity=%7B%22cid%22%3A%224248001%3A418282984%22%2C%22msgId%22%3A%221932305012288%22%7D&bizType=im&open_id=418282984">
## 访问页面
<img src="https://static.dingtalk.com/media/lALPDgQ9rTli8g7NA7HNBsU_1733_945.png_620x10000q90g.jpg?auth_bizType=IM&auth_bizEntity=%7B%22cid%22%3A%224248001%3A418282984%22%2C%22msgId%22%3A%221922113863507%22%7D&bizType=im&open_id=418282984">

## 地理分布/浏览器/操作系统/分辨率
<img src="https://static.dingtalk.com/media/lALPBE1XYqtzew_NAsbNBNc_1239_710.png_620x10000q90g.jpg?auth_bizType=IM&auth_bizEntity=%7B%22cid%22%3A%224248001%3A418282984%22%2C%22msgId%22%3A%221925312109833%22%7D&bizType=im&open_id=418282984">
## 特点:js错误诊断/用户操作追溯/再现错误
<img src="https://static.dingtalk.com/media/lALPDgQ9rTo8vE7NA1DNBo4_1678_848.png_620x10000q90g.jpg?auth_bizType=IM&auth_bizEntity=%7B%22cid%22%3A%224248001%3A418282984%22%2C%22msgId%22%3A%221933265446234%22%7D&bizType=im&open_id=418282984">
## 特点:自定义统计
-   求和统计：用于统计业务中某些事件发生的次数总和，例如某个按钮被点击的次数、某个模块被加载的次数等。

-   均值统计：用于统计业务中某些事件发生的平均值，例如某个模块加载的平均耗时等。

#### 求和统计 API

在页面中引入前端监控 SDK 后，在业务 JavaScript 文件中使用以下日志上报 API 进行求和统计。

调用参数： `__bl.sum(key, value)`

调用参数说明：

|参数|类型|描述|是否必需|默认值|
|--|--|--|----|---|
|key|String|事件名|是|-|
|value|Number|单次累加上报量，默认 1|否|1|

示例：

```
__bl.sum('event-a');
__bl.sum('event-b', 3);

```
## 特点:告警
<img src="https://static.dingtalk.com/media/lALPDgQ9rTo8vxfNAxzNA0I_834_796.png_620x10000q90g.jpg?auth_bizType=IM&auth_bizEntity=%7B%22cid%22%3A%224248001%3A418282984%22%2C%22msgId%22%3A%221922961906788%22%7D&bizType=im&open_id=418282984">