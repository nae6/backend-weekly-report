# バージョン履歴

このドキュメントでは、Backend Weekly Report のリリース履歴を管理する。

---

# Version 1.0.0

**リリース日**

2026-08-15

**ステータス**

Released

## 概要

Backend Weekly Report の初回リリース。

n8n・OpenAI API・Notion API を利用し、バックエンド関連の技術情報を毎週自動収集・分析し、初心者向けレポートを生成・保存するシステムを構築した。

---

## 実装内容

### ワークフロー

* Schedule Trigger
* RSS Read
* Merge
* Aggregate
* Edit Fields
* OpenAI
* Code
* Notion

---

### RSS

以下のRSSを採用した。

* PHP Official
* Laravel News
* Symfony Blog
* AWS What's New
* Docker Blog
* OpenAI Developer Blog
* Money Forward Developers
* LY Tech Blog

---

### OpenAI

以下を自動生成する。

* 今週最重要ニュース
* 技術トレンド
* Mermaid図（必要時のみ）
* 学習TODO
* 一言アドバイス

---

### Code Node

OpenAIの出力を約1800文字単位へ分割し、

Notion APIへ保存できる形式へ変換する。

---

### Notion

以下の情報を保存する。

* Title
* Date
* Report Type
* Priority
* Tags
* Report Body

---

## テスト

### 単体テスト

完了

* RSS
* Merge
* Aggregate
* Edit Fields
* OpenAI
* Code
* Notion

### E2Eテスト

完了

ワークフロー全体を実行し、

期待通りのレポートがNotionへ保存されることを確認した。

---

## Version 1.1

予定機能

* ロールモデル分析
* 技術トレンド分析
* 学習計画生成

Version 1.0で生成したBackend Weekly Reportを入力として利用する。

---

# 今後のVersion管理

今後は以下のルールでVersionを管理する。

## Patch

例

```text
v1.0.1
```

バグ修正のみ。

---

## Minor

例

```text
v1.1.0
```

機能追加。

---

## Major

例

```text
v2.0.0
```

大きな仕様変更。

---

以後、新しいVersionをリリースするたびに、このドキュメントへ変更内容を追記する。
