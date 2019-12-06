## 合成监控

### 什么叫合成监控？
就是在一个模拟场景里，去提交一个需要做性能审计的页面，通过一系列的工具、规则去运行你的页面，提取一些性能指标，得出一个审计报告。

合成监控中最近比较流行的是 Google 的 Lighthouse。

link:https://developers.google.com/web/tools/lighthouse

<img src="https://static.geekbang.org/infoq/5c6cf6b49f913.png">

#### lighthouse 使用方法
环境：安装 Node，需要版本 5 或更高版本。
```
npm install -g lighthouse
```
针对一个页面运行 Lighthouse 审查。
```
lighthouse https://baidu.com/
```
Lighthouse 提供了很多种方案，这里演示的是一种以命令行工具的形式去对一个baidu首页做性能审计。我们可以输出目标页面，它就会在我们这个模拟环境里面打开baidu，然后去做一些性能数据的提取。

<img src="https://static.dingtalk.com/media/lALPDgQ9rTx0NqnNA9vNBBE_1041_987.png?auth_bizType=IM&auth_bizEntity=%7B%22cid%22%3A%224248001%3A418282984%22%2C%22msgId%22%3A%221935473233129%22%7D&bizType=im&open_id=418282984">

<img src="https://static.dingtalk.com/media/lALPDgQ9rTx8YBzNA7DNBD0_1085_944.png_620x10000q90g.jpg?auth_bizType=IM&auth_bizEntity=%7B%22cid%22%3A%224248001%3A418282984%22%2C%22msgId%22%3A%221935473462174%22%7D&bizType=im&open_id=418282984">

我们会看到一个生成的性能报告，从这个性能报告里我们可以看到，Lighthouse 生成的不仅仅是一些性能相关的数据，甚至包括 PWA，然后一些 SEO，甚至一些前端工程化的最佳实践等等。

<img src="https://static.dingtalk.com/media/lALPDgQ9rTyBQnTNAdzNA8Q_964_476.png_620x10000q90g.jpg?auth_bizType=IM&auth_bizEntity=%7B%22cid%22%3A%224248001%3A418282984%22%2C%22msgId%22%3A%221927744137262%22%7D&bizType=im&open_id=418282984">

<img src="https://static.dingtalk.com/media/lALPDgQ9rTyCGo3NAanNA6w_940_425.png_620x10000q90g.jpg?auth_bizType=IM&auth_bizEntity=%7B%22cid%22%3A%224248001%3A418282984%22%2C%22msgId%22%3A%221927696376643%22%7D&bizType=im&open_id=418282984">

### 合成监控优缺点


|优点|缺点|
|--|--|
|实现简单，解决方案成熟|无法全部还原真实场景|
|可以采集丰富数据，硬件指标或瀑布图|登录等场景需要额外解决|
|不影响真实用户|单次运行数据不够稳定|