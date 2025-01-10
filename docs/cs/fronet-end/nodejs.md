---
title: Node.js
date: 2024/02/17
math: true
categories:
  - [计算机科学, 前端]
tags:
  - 前端
  - Node.js
---

#### require

`require`是 Node.js 中用于加载模块的函数。它的主要作用是将外部模块引入到当前的 JavaScript 文件中，以便在代码中使用被引入的模块的功能、变量或对象。

`require`函数的基本语法是：

```javascript
require(moduleName);
```

其中，`moduleName`是要引入的模块的名称或路径。

`require`函数的工作原理如下：

1. 首先，Node.js 会检查`moduleName`是否是一个核心模块（Node.js 内置的模块）。如果是核心模块，Node.js 会直接加载并返回该模块的导出内容。
2. 如果`moduleName`不是核心模块，Node.js 会尝试在当前目录下查找指定名称的文件或文件夹。
   - 如果指定名称是一个文件，Node.js 将尝试加载该文件，并返回文件中导出的内容。
   - 如果指定名称是一个文件夹，Node.js 将尝试查找该文件夹下的`package.json`文件。如果找到了`package.json`文件，并且文件中包含一个`"main"`属性，Node.js 将加载该属性指定的文件，并返回文件中导出的内容。
   - 如果以上两种情况都不满足，Node.js 将尝试查找指定名称的文件夹下的`index.js`文件，并返回文件中导出的内容。
3. 如果在当前目录下找不到指定的文件或文件夹，Node.js 会递归地向上查找，直到找到指定的模块或到达文件系统的根目录为止。如果在任何一个父级目录中找到了指定的模块，Node.js 将加载该模块并返回导出的内容。
4. 如果在所有的搜索路径中都找不到指定的模块，Node.js 将抛出一个`Error`。

需要注意的是，`require`函数是同步的，并且模块在第一次被引入后会被缓存，所以多次调用`require`函数引入同一个模块不会导致模块被重复加载，而是直接返回缓存的内容。如果想要重新加载一个模块，可以使用`delete require.cache[moduleName]`来删除缓存。

#### HTTP 请求

在现代的 Node.js 环境中，进行 HTTP 请求最广泛使用且比较前沿的模块是`node-fetch`和`axios`。它们都支持 TypeScript，并提供了简洁的 API 来发送 HTTP 请求。

1. **node-fetch**：`node-fetch`是一个基于 Fetch API 标准的轻量级 HTTP 客户端。它提供了类似于浏览器中`fetch`函数的接口，可以发送 HTTP 请求并处理响应。`node-fetch`在 Node.js 环境中非常流行，因为它支持 Promise，并且使用起来非常简洁。你可以使用以下命令安装`node-fetch`模块：

   ```
   npm install node-fetch
   ```

   或者使用 yarn：

   ```
   yarn add node-fetch
   ```

   安装完成后，你可以在 TypeScript 代码中引入并使用`node-fetch`模块来发送 HTTP 请求。

2. **axios**：`axios`是一个流行的基于 Promise 的 HTTP 客户端，可以在 Node.js 环境和浏览器中使用。它提供了简洁的 API 和许多功能，如请求拦截器、响应拦截器、取消请求等。`axios`在 Node.js 环境中也非常受欢迎，并且有广泛的社区支持。你可以使用以下命令安装`axios`模块：
   ```
   npm install axios
   ```
   或者使用 yarn：
   ```
   yarn add axios
   ```
   安装完成后，你可以在 TypeScript 代码中引入并使用`axios`模块来发送 HTTP 请求。

这两个模块都非常流行和可靠，具体使用哪个取决于你的个人偏好和项目要求。它们都提供了良好的文档和示例，以帮助你开始使用它们进行 HTTP 请求。
