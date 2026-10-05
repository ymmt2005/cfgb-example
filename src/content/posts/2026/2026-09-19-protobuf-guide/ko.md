---
title: Protocol Buffers 스키마 읽기
slug: reading-protobuf-schemas
publishedAt: '2026-09-20T09:00:00+09:00'
topics:
- protobuf
- oss
summary: 작은 Protocol Buffers 메시지에서 필드 번호를 확인하고 타입 참조를 따라갑니다. 이 다국어 예제는 공통 다이어그램을 사용해 스키마 변경 시 고려할 호환성 문제를 소개합니다.
ogImage: ./assets/schema.svg
---
## 메시지부터 시작하기

Protocol Buffers 스키마는 메시지와 필드를 정의합니다. 타입 참조를 따라가면 API가 반환하는 데이터 구조를 이해할 수 있습니다.

![메시지는 번호가 지정된 필드를 포함합니다](./assets/schema.svg)

### 필드 번호

이름은 읽는 사람의 이해를 돕고, 필드 번호는 호환성을 논의하는 데 필요한 정보입니다.

```proto title="user.proto" {3}
syntax = "proto3";
message User {
  string display_name = 1;
}
```

## 더 읽어 보기

https://github.com/ymmt2005/pbschema-lens

[Markdown 예제](../2026-09-20-markdown-showcase/ko.md#코드)도 참고하세요.
