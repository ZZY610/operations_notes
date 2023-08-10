# JSON
JavaScript Object Notation(JavaScript 对象表示法)

## xml与json
- XML（可扩展标记语言，Extensible Markup Language）和JSON（JavaScript对象表示法，JavaScript Object Notation）是两种常用的数据交换格式。它们都可以用于表示结构化数据，但在语法和使用场景上有一些不同。
1. 概念：

XML：
XML 是一种通用的数据格式，用于表示层次结构和关系的数据。它是由一系列标签（tag）和属性（attribute）组成的，标签用于定义数据的结构，属性用于提供有关数据的详细信息。

JSON：
JSON 是一种轻量级的数据格式，它是一种基于文本的数据表示方法，用于描述对象、数组、字符串、数字、布尔值和 null。JSON 语法源于 JavaScript 对象表示法，但它已成为一种独立的数据交换格式，可以在许多编程语言中使用。
1. 联系：

XML 和 JSON 都是用于表示结构化数据的数据格式，它们可以在不同的应用程序和编程语言之间进行数据交换。
1. 区别：
- 语法：XML 使用标签和属性来表示数据，而 JSON 使用键值对表示数据。XML 的语法较为繁琐，而 JSON 的语法更为简洁。
- 可读性：JSON 相对于 XML 更易于阅读和编写。
- 数据类型：XML 没有内置的数据类型，所有数据都是文本形式。JSON 支持多种数据类型，如字符串、数字、布尔值和 null。
- 文件大小：由于 XML 的语法繁琐，XML 文件通常比 JSON 文件大。在网络传输中，JSON 更节省带宽。
- 解析速度：JSON 通常比 XML 更容易解析，因为许多编程语言都有内置的 JSON 解析器，而解析 XML 需要额外的库或工具。
1. 格式：

XML 示例：

```xml

<person>
  <name>John Doe</name>
  <age>30</age>
  <city>New York</city>
</person>
```



JSON 示例：

```json

{
  "name": "John Doe",
  "age": 30,
  "city": "New York"
}
```


1. 用法：

XML 主要用于文档表示和数据交换，如配置文件、SOAP（一种基于 XML 的 Web 服务协议）等。

JSON 主要用于 Web 应用程序的数据交换，尤其在前后端分离的应用程序中。JSON 通常与 RESTful API（一种基于 HTTP 的轻量级 Web 服务协议）一起使用。

总之，XML 和 JSON 都是用于表示和传输结构化数据的数据格式，但它们在语法、可读性、文件大小等方面有所不同。JSON 更适合 Web 应用程序的数据交换，而 XML 在一些特定场景下（如文档表示和复杂的数据交换）仍然具有优势。