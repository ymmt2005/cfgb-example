---
title: 阅读 Protocol Buffers 模式
slug: reading-protobuf-schemas
publishedAt: '2026-09-20T09:00:00+09:00'
topics:
- protobuf
- oss
summary: 从一个小型 Protocol Buffers 消息入手，识别字段编号并追踪类型引用。这个多语言示例使用共享的关系图，介绍修改模式时需要考虑的兼容性问题。
ogImage: ./assets/schema.svg
---
## 从消息开始

Protocol Buffers 模式定义消息及其字段。沿着类型引用阅读，可以理解 API 返回的数据结构。

![消息包含带编号的字段](./assets/schema.svg)

### 字段编号

名称帮助读者理解含义，而字段编号是讨论兼容性时的重要信息。

```proto title="user.proto" {3}
syntax = "proto3";
message User {
  string display_name = 1;
}
```

## 延伸阅读

https://github.com/ymmt2005/pbschema-lens

也可以参阅 [Markdown 展示](../2026-09-20-markdown-showcase/zh-Hans.md#代码)。
