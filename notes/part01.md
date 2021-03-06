## 模块化开发

> 当下最重要的前端开发范式
> 1. 前端应用日益复杂, 代码膨胀
> 2. 模块化开发是一种代码组织方式
> 3. 将复杂代码 按功能不同 划分不同模块 单独维护
> 4. 模块化只是思想

> 内容概要
> 1. 模块化演变过程
> 2. 模块化规范
> 3. 常用的模块化打包工具
> 4. 基于模块化工具构建现代的web应用
> 5. 打包工具的优化技巧


### 1. 模块化的演变过程

> stage-1 文件划分方式
> 1. 污染全局作用域
> 2. 命名冲突
> 3. 无法管理模块依赖关系
> 4. 完全依靠约定, 项目庞大后力不从心

> stage-2 命名空间方式
> 在第一阶段基础上, 将每一模块内的成员包裹成一个对象, 对外暴露该对象
> 1. 减少命名冲突的可能
> 2. 没有私有空间, 模块成员可以被修改

> stage-3 IIFE (立即执行函数的方式, 为模块提供私有空间)
> 1. 将每个成员都放在一个函数提供的私有作用域中, 确保了私有变量的安全
> 2. 需要对外暴露的成员, 通过挂载到全局对象的方式暴露
> 3. 将函数的参数作为依赖声明使用, 使得依赖关系显式显现

> 早期在没有工具和规范的情况下对模块化的落地方式

> 模块化规范的出现

> 模块化标准 + 模块加载器

> CommonJS 规范
> 1. 一个文件就是一个模块
> 2. 每个模块都有单独的作用域
> 3. 通过 module.exports 导出成员
> 4. 通过 require 函数载入模块
> 5. CommonJS 是以同步模式加载模块, 浏览器端使用CommonJS规范会有不便
> 6. 前端AMD规范 define定义模块 require加载模块
> 7. AMD  使用相对复杂, 模块JS文件请求频繁
> 8. sea.js + CMD  (阿里)

> 模块化标准规范
> 最佳实践方式

> nodeJS -> CommonJS

> 浏览器环境中 -> ES Modules (ES6 出现的规范)

### 2. ES Modules

> 通过给script 添加 type = module 的属性, 就可以以 ES Module 的 标准执行其中的 js 代码

```js
    <script type='module'>
        console.log('this is es module') 
    </script>
```
> 1. ESM 自动采用严格模式, 忽略 'use strict', 不能再全局范围使用this, this == undefind , 普通 this == window
> 2. 每个 ES Module 都是运行在单独的私有作用域中
> 3. ESM 是用过 CORS 的方式请求外部 JS 模块的 
> 4. ESM 的 script 标签会延迟执行脚本 == script 使用 defer 属性

##### 1. ES Modules 导入和导出
```js
    // export 导出
    // ./modules.js
    const foo = 'es modules'
    export {foo}

    // import 导入
    // ./app.js
    import {foo} from './module.js'
    console.log(foo) // => es modules

    // 使用 as 重命名
    export{
        foo as fooName
    }
    import {fooName} from './module.js'

    // 特殊
    // 如果导出成员名称 为 default , 导入时必须重命名
    export{
        foo as default
    }
    import {
        default as fooName
    } from './module.js'

    // 默认导出特殊用法
    export default foo
    import fooName from './module.js'
```

```js
    // 导入导出 注意事项
    // 1. exprot 导出的不是对象的字面量  import 导入的不是对象的结构
    // export 和 import 后的花括号 只是固定用法
    var name = "jack";
    var age = 18;
    var obj = {name, age}
    export{ name, age}
    import{name, age}
    // export default {name, age} 这个时候才是导出对象
    // import {name, age } 会报错
    // 需要 import tempObj from ""

    // 2. export 导出的是 引用关系 内存地址 只读的属性
    // 也就是模块内部可以修改 name 和 age 的值， 会对外部造成影响
    // 外部不能修改 name 和 age 的值
```

```js
    // import 导入
    import {name} from './modules.js';
    // from 后是完整路径, 不能省略扩展名  与 commonJs 区别
    // 可以使用绝对路径,相对路径,请求路径   允许直接引用cdn 上的包
    import {name} from '/fed-e-task-02-02/notes/modules.js'
    import {name} from 'http://localhost:3000/fed-e-task-02-02/notes/modules.js'
    // ./不能省略  省略会认为在应用第三方库
    // import {name} from "modules.js"

    // import {} from "./modules.js"
    // 执行模块, 不提取任何成员
    // 简写方式 import "./modules.js"

    // import * as mod from "./modules.js"
    // 导出全部成员, 已mod对象的方式导出, 以mod.name 的方式读取和使用

    // 动态导入模块
    var modulePath = "./modules.js";
    import (modulePath).then(function(modules) {
        console.log(modules)
    })

    // 同时导出匿名成员 和 默认成员
    import {name, age, default as title} from './modules.js';
    // 简写
    import title, {name, age} from './modules.js';

```

```js
    // 直接导出导入成员
    // import 替换成 export
    export{name, age} from './modules.js';
    // 直接将导入成员导出
    // 使用 index.js 将大量导入 批量导出
```


```js
    

```