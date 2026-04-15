---
title: "作成したps1の実行方法"
emoji: "💭"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["ps1"]
published: false
---
windowsではユーザーが作成したps1をデフォルトで実行することはできない。

```bash
powershell -ExecutionPolicy Bypass -File 作成したps1のパス
```
で実行できる