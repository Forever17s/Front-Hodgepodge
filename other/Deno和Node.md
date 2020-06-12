<div align="center">

## Deno Vs Node

  <img src="./../resource/other_node_and_deno.png" width="500" />

</div>

#### Deno 吸引开发者的重要原因

##### Deno 内置 API

Deno 并未像 Node 一样使用引入模块的方式，而是采用全局的 Deno 对象。[Deno 内置 API 文档](https://doc.deno.land/https/github.com/denoland/deno/releases/latest/download/lib.deno.d.ts):point_left:

```javascript
// node 模块导入
const fs = require("fs");
fs.readFileSync("./data.txt");

// deno 内置 API
Deno.readFileSync("./data.txt");
```

##### 现代化的 Javascript：ES 模块

```javascript
// 类似 node 中的 uuid 包
import { v4 } from "https://deno.land/std/uuid/mod.ts";
```

Deno 的 ES 模块 import 有两个优势：

- 通过 `import`，你可以有选择地从包中加载所需的部件，从而节约了内存空间。
- 加载与 `require` 是 **同步** 的，而 `import` 则会 **异步** 加载模块，从而提高了性能。

> 如上示例，我们可以从一个 URL 中直接导入 v4 包，这也是 Deno 的另一个优势。

##### 去中心化包

`Deno` 不用再依赖 `NPM` 了。是的，不再需要 `package.json`。可以从一个 `URL` 直接加载（可能有网络、url 反复嵌套、依赖的版本问题，看作者后期如何规范化吧）。
每个包安装完成后都缓存在硬盘驱动器上。也就是说软件包的安装过程只运行一次。要在任何地方再次导入依赖项时，并不需要重新下载。

```javascript
// 支持
import * as fs from "https://deno.land/std/fs/mod.ts";
import { deepCopy } from "./deepCopy.js";
import foo from "/foo.ts";

// 不支持
import foo from "foo.ts";
import bar from "./bar"; // 必须指定扩展名
```

##### TypeScript 原生支持，无需配置

- 在 `Node` 中使用 `TypeScript` 需要很多准备工作。你必须安装 `typescript`，更新 `package.json`、`tsconfig.json`，并确保你的模块支持 `@types`。

- 在 `Deno` 中，你要做的就是将文件另存为`.ts` 而不是`.js`，`TypeScript` 编译器已经准备就绪了。

##### 顶级 await：在异步函数外使用 await

在 `Deno` 中，你可以随时随地 `await` 任何事情

```javascript
// node 回调方式
const fs = require("fs");
fs.readFile("./data.txt", (err, data) => {
  if (err) throw err;
  console.log(data);
});

// node 改进后promisify API方式
const { promisify } = require("es6-promisify");
const fs = require("fs");

async function main() {
  const readFile = promisify(fs.readFile);
  const data = await readFile("./data.txt");
  console.log(data);
}
main();

// deno Promise方式
const data = await Deno.readFile("./data.txt");
console.log(data);
```

##### 标准模块

`Deno` 为开发者提供了一个没有外部依赖的、实用的、高频的开发库，减轻我们开发的负担：

- **node**: node API 兼容模块；
- **io**: 二进制读写操作；
- **http**: 网络和 web 服务相关；
- **path**: 文件路径相关；
- **colors**: 输出有颜色的文字，类似 chalk 库；
- **printf**: 格式化输出，类似 C 语言的 printf；
- **tar**: 解压与压缩；
- **async**: 生成异步函数的；
- **bytes**: 二进制比较和查找等；
- **datetime**: 日期相关；
- **encoding**: 文本的与二进制的转化、CSV 和对象转化、yarml 和对象转化等；
- **flags**: 命令行参数解析；
- **hash**: 字符转 sha1 和 sha256；
- **fs**: 文件系统模块，类似 node 的 fs 模块；
- **log**: 日志管理；
- **permissions**: 权限相关；
- **testing**: 测试和断言相关；
- **uuid**: 用于生成 UUID；
- **ws**: WebSocket 相关；
- :dart::dart::dart:不断的完善和扩充中

###### 相关资料

[20 分钟入门 deno](https://juejin.im/post/5ebcabb2e51d454da74185a9#heading-11)
[JavaScript 开发人员更喜欢 Deno 的五大原因](https://mp.weixin.qq.com/s/7pqDSRRCz0K17gPW7hVt8w)
[Deno 正式发布，彻底弄明白和 node 的区别](https://www.v2ex.com/t/671814)
