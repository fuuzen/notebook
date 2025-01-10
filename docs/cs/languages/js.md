---
title: JavaScript基础
date: 2024/02/17
math: true
categories:
  - [计算机科学, 编程语言]
tag:
  - JS
  - 前端
---

#### JSON.stringify()

`JSON.stringify()` 是 JavaScript 中的一个方法，用于将 JavaScript 对象或值转换为 JSON 字符串。

具体作用如下：

- 序列化：`JSON.stringify()` 方法将 JavaScript 对象或值序列化为一个 JSON 字符串。序列化是指将对象的结构和数据转换为字符串的过程，使其可以在网络传输或存储中进行传递和持久化。
- JSON 格式：JSON（JavaScript Object Notation）是一种常用的数据交换格式，具有简洁、易读和跨语言的特性。`JSON.stringify()` 将 JavaScript 对象或值转换为符合 JSON 格式的字符串，方便与其他系统或服务进行数据交互。

语法：

```javascript
JSON.stringify(value[, replacer[, space]])
```

- `value`：要被序列化为 JSON 字符串的 JavaScript 对象或值。
- `replacer`（可选）：一个函数或数组，用于控制序列化过程中的属性过滤和转换。可以传递一个函数作为 replacer，该函数将被应用于对象的每个属性，并在序列化时对属性值进行转换。也可以传递一个数组，包含要序列化的属性名列表，仅包含在该列表中的属性将被序列化。
- `space`（可选）：用于美化输出的空格数。如果是一个数字，则表示使用相应数量的空格进行缩进；如果是一个字符串，则使用该字符串作为缩进符号。

下面是一个简单的示例：

```javascript
const obj = {
  name: "Alice",
  age: 30,
  isActive: true,
};

const jsonString = JSON.stringify(obj);
console.log(jsonString);
// 输出: '{"name":"Alice","age":30,"isActive":true}'
```

在上面的示例中，`obj` 对象通过 `JSON.stringify()` 方法被序列化为一个 JSON 字符串。`jsonString` 的值为 `{"name":"Alice","age":30,"isActive":true}`。该字符串符合 JSON 格式，可以在传输或存储中使用。

`JSON.stringify()` 方法用于将 JavaScript 对象或值转换为 JSON 字符串，方便数据的传输和持久化。

#### JSON.parse()

`JSON.parse()`是一个 JavaScript 内置函数，用于将符合 JSON 格式的字符串转换为对应的 JavaScript 对象。

`JSON.parse()`的基本语法如下：

```javascript
JSON.parse(jsonString);
```

其中，`jsonString`是一个符合 JSON 格式的字符串。

`JSON.parse()`函数的工作原理如下：

1. 首先，它会接收一个 JSON 格式的字符串作为输入。
2. 然后，它会解析这个字符串，并将其转换为对应的 JavaScript 对象。
3. 如果解析成功，它将返回转换后的 JavaScript 对象；如果解析失败，它将抛出一个`SyntaxError`。

在将字符串转换为 JavaScript 对象时，`JSON.parse()`支持的 JSON 数据类型有：

- 字符串（字符串需要使用双引号）
- 数字（整数或浮点数）
- 布尔值（`true`或`false`）
- `null`
- 数组（用方括号`[]`表示）
- 对象（用花括号`{}`表示）

以下是一些示例，展示了`JSON.parse()`函数的用法：

```javascript
const jsonString =
  '{"name": "John", "age": 30, "isStudent": true, "hobbies": ["reading", "swimming"]}';
const obj = JSON.parse(jsonString);
console.log(obj.name); // 输出: John
console.log(obj.age); // 输出: 30
console.log(obj.isStudent); // 输出: true
console.log(obj.hobbies[0]); // 输出: reading
console.log(obj.hobbies[1]); // 输出: swimming
```

在这个示例中，我们有一个符合 JSON 格式的字符串`jsonString`，然后我们使用`JSON.parse()`将其转换为了一个 JavaScript 对象`obj`。接下来，我们可以通过访问`obj`的属性来获取和操作 JSON 数据。

需要注意的是，`JSON.parse()`只能解析合法的 JSON 字符串，因此如果提供给它的字符串不符合 JSON 语法，就会导致解析失败并抛出`SyntaxError`。另外，`JSON.parse()`不支持解析带有函数、日期或正则表达式等特殊类型的 JSON 数据。

#### JSON.parse(JSON.stringify( ))

`JSON.parse(JSON.stringify())`函数组合用法可以将一个 JavaScript 对象进行深度克隆，从而创建一个与原对象完全相同的新对象。这个过程可以确保输入和输出是不变的。

`JSON.stringify()`函数将 JavaScript 对象转换为 JSON 字符串，而`JSON.parse()`函数将 JSON 字符串解析为 JavaScript 对象。通过将一个对象先使用`JSON.stringify()`转换为字符串，然后再使用`JSON.parse()`解析回对象，可以创建一个新的对象，该对象与原对象在值上是相等的，但是在引用上是不同的。

这个过程的效果是创建了原对象的一个深度副本（deep copy），而不是对原对象进行引用传递。这在需要确保对原对象的修改不会影响到副本时非常有用。

需要注意的是，这种深度克隆方法只适用于可以被 JSON 序列化和反序列化的数据类型，例如对象、数组、字符串、数字、布尔值等。对于无法被 JSON 序列化的数据类型，比如函数、正则表达式、`undefined`等，这种方法将无法克隆它们。

下面是一个示例，展示了`JSON.parse(JSON.stringify())`函数组合的效果：

```javascript
const obj = { a: 1, b: { c: 2 } };

const clone = JSON.parse(JSON.stringify(obj));

console.log(obj); // { a: 1, b: { c: 2 } }
console.log(clone); // { a: 1, b: { c: 2 } }

console.log(obj **= clone); // false
console.log(obj.b **= clone.b); // false
console.log(obj.b.c **= clone.b.c); // true
```

在上述示例中，`obj`对象被克隆为`clone`对象，它们的值是相等的，但是它们是两个不同的对象，因此对其中一个对象的修改不会影响到另一个对象。

#### Promise

Promise 是 JavaScript 中处理异步操作的一种机制，它代表了一个异步操作的最终完成或失败，并可以返回结果或错误。

在传统的 JavaScript 中，处理异步操作通常使用回调函数。但是，回调函数的嵌套会导致代码变得难以理解和维护，形成所谓的"回调地狱"。为了解决这个问题，Promise 被引入作为一种更优雅和可读性更高的异步编程模式。

一个 Promise 对象代表了一个异步操作，它有三种状态：

1. **Pending（进行中）**：初始状态，表示异步操作还未完成，仍处于进行中的状态。

2. **Fulfilled（已成功）**：表示异步操作已经成功完成，并返回了一个值。在这种状态下，Promise 对象会调用`then`方法注册的回调函数，并将异步操作的结果传递给这些回调函数。

3. **Rejected（已失败）**：表示异步操作发生了错误或失败。在这种状态下，Promise 对象会调用`catch`方法注册的回调函数，并将错误信息传递给这些回调函数。

Promise 对象具有以下特点：

- Promise 对象是不可变的，一旦进入了 Fulfilled 或 Rejected 状态，就不会再发生状态的改变。
- 通过调用`then`方法可以注册成功回调函数，通过调用`catch`方法可以注册失败回调函数。
- Promise 对象可以被链式调用，通过返回新的 Promise 对象来实现链式操作。
- Promise 提供了一些静态方法，如`Promise.all`、`Promise.race`等，用于处理多个 Promise 对象。

在现代的 JavaScript 中，Promise 已经成为了标准的异步操作处理方式，被广泛应用于各种场景，如网络请求、文件操作等。它提供了一种更直观、可靠和易于使用的方式来处理异步操作，避免了回调地狱问题，提高了代码的可读性和可维护性。

### 网络请求处理

#### handler(req, res)

`async function handler(req, res)` 是一个异步函数，通常用作处理 HTTP 请求的处理程序。这个函数接受两个参数 `req` 和 `res`，分别代表请求对象和响应对象。

具体的函数实现和参数可能会根据具体的需求和开发框架而有所不同。在某些情况下，`req` 和 `res` 参数可能会被命名为其他名称，而函数的实现也可能会有其他逻辑。

以下是一个示例，展示了一个类似的函数实现，用于处理 HTTP POST 请求，返回一个 JSON 响应：

```javascript
// handler.js

export default async function handler(req, res) {
  try {
    const { body } = req;

    // 处理请求逻辑
    const result = await processRequest(body);

    // 返回 JSON 响应
    res.status(200).json({ success: true, data: result });
  } catch (error) {
    console.error(error);
    res.status(500).json({ success: false, error: "Internal Server Error" });
  }
}
```

在上述示例中，`handler` 函数是一个异步函数，它接受 `req` 和 `res` 作为参数。函数内部可以根据具体需求进行逻辑处理，例如从请求体中获取数据、调用其他函数进行处理，并最终返回一个 JSON 响应。

#### axios

##### axios.post

`await axios.post`是使用 axios 发送 POST 请求并等待其响应的语法。使用`await`关键字可以让 JavaScript 在发送请求后暂停执行，直到获得响应为止。

下面是一个使用`await axios.post`发送 POST 请求的示例：

```typescript
import axios from "axios";

async function postData() {
  try {
    const response = await axios.post("https://api.example.com/data", {
      name: "John",
      age: 30,
    });

    console.log(response.data);
  } catch (error) {
    console.error(error);
  }
}

postData();
```

在这个示例中，我们定义了一个名为`postData`的异步函数。在该函数内部，我们使用`await axios.post`发送 POST 请求到指定的 URL，并传递一个包含数据的 JavaScript 对象作为请求体。

当发送请求时，JavaScript 会等待 axios 返回响应。一旦收到响应，它会将响应存储在`response`变量中。我们可以使用`response.data`来访问响应数据。

如果请求成功，我们打印出响应数据。如果请求失败，则会捕获错误并打印错误信息。

需要注意的是，在使用`await`关键字之前，必须将其包裹在`async`函数内部。这是因为`await`只能在异步函数中使用。

!!! warning
    在使用`await`时，必须将其放在`async`函数内部。这样才能正确地使用`await axios.post`并处理响应。

`axios.post`是一个用于发送 HTTP POST 请求的方法，它返回一个 Promise 对象。这个 Promise 对象在请求完成后会被解析，提供了请求的响应数据和其他相关信息。

`await axios.post`的返回值是一个 Promise，它解析为 axios 的响应对象。响应对象具有许多属性，其中最常用的是`data`属性，它包含服务器返回的数据。具体地说，`axios.post`方法返回的 Promise 对象具有以下结构：

```typescript
interface AxiosResponse<T = any> {
  data: T;
  status: number;
  statusText: string;
  headers: any;
  config: AxiosRequestConfig;
  request?: any;
}

interface AxiosError<T = any> extends Error {
  config: AxiosRequestConfig;
  code?: string;
  request?: any;
  response?: AxiosResponse<T>;
  isAxiosError: boolean;
}

interface AxiosPromise<T = any> extends Promise<AxiosResponse<T>> {}
```

- `AxiosResponse` 是成功响应的类型，它包含以下属性：

  - `data`：响应的数据，类型为泛型`T`所指定的类型。
  - `status`：HTTP 响应状态码。
  - `statusText`：HTTP 响应状态文本。
  - `headers`：响应的 HTTP 头信息。
  - `config`：发起请求时的配置对象。
  - `request`：发起的请求实例。

- `AxiosError` 是请求错误时的类型，它继承自`Error`对象，包含以下属性：

  - `config`：发起请求时的配置对象。
  - `code`：请求错误的错误码。
  - `request`：发起的请求实例。
  - `response`：包含错误响应的`AxiosResponse`对象。
  - `isAxiosError`：标识是否为 Axios 错误。

- `AxiosPromise` 是`axios.post`方法返回的 Promise 对象的类型，它是一个泛型类型，泛型`T`表示响应数据的类型。

通过对返回的 Promise 对象调用`.then`方法，您可以注册成功回调函数来处理响应数据。如果请求失败，则可以通过`.catch`方法注册错误回调函数进行错误处理。

示例代码：

```typescript
axios
  .post(url, data)
  .then((response: AxiosResponse) => {
    // 处理成功响应
    console.log(response.data);
  })
  .catch((error: AxiosError) => {
    // 处理错误
    console.error(error.message);
  });
```

请注意，`axios.post`返回的 Promise 对象根据请求结果可能会被解析为成功响应或错误响应。因此，在处理响应时，建议使用`.then`和`.catch`方法来适当地处理成功和错误的情况。

### 网络请求

#### fetch( )

使用 JavaScript 内置的 Fetch API 进行网络请求：

1. 构建请求参数：创建一个包含请求方法、请求头、请求体等信息的配置对象。

2. 发起网络请求：使用 `fetch()` 函数发送请求，并将配置对象作为参数传递给它。`fetch()` 函数返回一个 Promise 对象，代表异步操作的结果。

3. 处理响应：通过 Promise 的解决来获取服务器返回的响应。可以使用响应对象的方法和属性来读取响应的状态、头部和主体数据。

4. 解析响应数据：根据服务器返回的数据类型，使用合适的方法将响应的主体数据解析为可用的格式，例如 JSON、文本或二进制数据。

下面是一个使用 Fetch API 进行 GET 请求的示例：

```javascript
fetch("https://api.example.com/data")
  .then((response) => {
    if (!response.ok) {
      throw new Error("Network response was not ok");
    }
    return response.json();
  })
  .then((data) => {
    console.log(data); // 处理响应数据
  })
  .catch((error) => {
    console.error("Error:", error);
  });
```

上述示例展示了一个简单的 GET 请求，其中：

- 使用 `fetch()` 函数发送了一个 GET 请求到 `'https://api.example.com/data'`。
- `fetch()` 函数返回的 Promise 对象通过 `.then()` 方法进行链式处理。在第一个 `.then()` 中，我们检查响应的状态码是否为成功状态（`ok`），如果不是，则抛出一个错误。
- 如果响应状态码正常，我们使用 `response.json()` 方法将响应的主体数据解析为 JSON 格式。
- 最后一个 `.then()` 用于处理解析后的数据。
- 如果在请求过程中发生错误，或任何一个 `.then()` 中抛出了错误，将进入 `.catch()`，在控制台输出错误信息。

类似地，你可以使用 `fetch()` 函数进行 POST 请求，只需在配置对象中设置请求方法为 `'POST'`，并通过 `body` 属性提供请求体数据。

请注意，Fetch API 默认不会将错误状态（如 404 或 500）视为网络错误。它会将这些状态码视为请求成功，并通过 `response.ok` 属性进行判断。如果你希望将这些状态码也视为网络错误，可以在第一个 `.then()` 中进行额外的检查或自定义处理逻辑。

这是使用 JavaScript 内置的 Fetch API 进行网络请求的基本流程。根据具体的需求和场景，你可以进一步使用其他方法和属性来扩展和定制请求和响应的处理。
