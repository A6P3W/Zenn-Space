---
title: Terraform 実行環境における数値オーバーフロー
emoji: 📝
type: tech
topics:
  - terraform
published: false
---

## 1. 概要

`terraform plan` 実行時、Google Cloud Providerプラグインがパニック（強制終了）を起こし、実行が中断された。Terraform本体のアーキテクチャ（32bit版）に起因する数値パースエラーであることが判明した。

## 2. 発生事象

### エラーメッセージ

```
Planning failed. Terraform encountered an error while generating this plan.

│ Error: Plugin did not respond
│ The plugin encountered an error, and failed to respond to the plugin.(*GRPCProvider).ReadResource call. The plugin logs may contain more details.

panic: Error reading level state: strconv.ParseInt: parsing "1777010473449967": value out of range
Error: The terraform-provider-google_v5.45.2_x5.exe plugin crashed!
```

### 発生した問題

- 特定のリソース（`google_storage_bucket_object.function_source`）の状態読み取り中にプラグインがクラッシュ。

- `terraform init -upgrade` によるプロバイダーの更新、および特定バージョン（v5.45.1/v5.45.2）への固定を試みたが、いずれも同様のクラッシュが発生し解決に至らなかった。

## 3. 原因分析

### 根本原因

Terraformの実行バイナリが **32bit版（windows_386）** であったこと。

1. GCP側から返却されたオブジェクトの世代管理ID（Generation ID等）が `1777010473449967` という巨大な数値であった。
2. 32bit環境における符号付き整数の最大値は約21億（$2^{31}-1$）である。
3. プロバイダー内部で `strconv.ParseInt` を用いてこの数値を処理しようとした際、32bit整数の範囲を大幅に超えていたため、Go言語のランタイムが「範囲外（value out of range）」としてパニックを発生させた。

## 4. 対応内容

- **バイナリの置換:** 32bit版（windows_386）のTerraformを、64bit版（**windows_amd64**）に差し替えた。

### 実行結果

- `terraform version` にて `on windows_amd64` であることを確認。
- 64bit環境では `strconv.ParseInt` が 64bit整数（int64）として正しくパースを行うため、同一の数値データを正常に処理でき、解決した。