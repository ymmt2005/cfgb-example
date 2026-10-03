---
title: Protocol Buffers のスキーマを読む
slug: protobuf-schema-guide
publishedAt: '2026-09-19T13:12:40+09:00'
topics:
- protobuf
- oss
summary: Protocol Buffers のメッセージ、フィールド番号、型の参照を小さな例で確認します。スキーマの関係図と Go のコードを使い、変更時に確認したい互換性の観点を整理する、日本語と英語の対訳サンプルです。
ogImage: ./assets/schema.svg
aliases:
- /ja/posts/old-protobuf-guide/
---
## スキーマの入口

Protocol Buffers のスキーマは、メッセージとフィールドの関係を定義します。型の参照をたどると、API が返すデータの形を確認できます。

![メッセージとフィールドの関係](./assets/schema.svg)

### フィールド番号

フィールド番号は互換性を考える手がかりです。このサンプルでは名前と番号を分けて表示します。

```proto title="user.proto" {3}
syntax = "proto3";
message User {
  string display_name = 1;
}
```

## 関連する資料

次の単独 URL はキャッシュ済みのリンクカードになります。

https://github.com/ymmt2005/pbschema-lens

[Markdown 表現のサンプル](../2026-09-20-markdown-showcase/ja.md#コード)も参照してください。
