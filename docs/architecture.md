# システム構成書（Architecture）

## 1. 概要

Backend Weekly Report は、バックエンド技術に関する最新情報を毎週自動収集し、OpenAIで重要度を判断したうえで、初心者向けのレポートを生成し、Notionへ保存するシステムである。

ワークフローは **n8n** を利用して構築し、毎週土曜日20:00に自動実行される。

---

# 2. システム構成

```text
Schedule Trigger
        │
        ▼
RSS取得
（8サイト）
        │
        ▼
Merge
        │
        ▼
Aggregate
        │
        ▼
Edit Fields
        │
        ▼
OpenAI
        │
        ▼
Code
        │
        ▼
Notion
```

---

# 3. 使用サービス

| サービス | 役割 |
| ---------- | ----------- |
| n8n | ワークフロー制御 |
| RSS | 最新記事取得 |
| OpenAI API | 記事分析・レポート生成 |
| Notion API | レポート保存 |

---

# 4. RSS情報源

## 公式情報

* PHP Official
* Laravel News
* Symfony Blog
* AWS What's New
* Docker Blog
* OpenAI Developer Blog

## 企業Tech Blog

* Money Forward Developers
* LY Tech Blog

### 選定方針

以下の基準で情報源を選定している。

* 公式情報を優先する
* 実務で利用頻度が高い技術を対象とする
* 初学者に価値のある情報を優先する
* 情報の重複が少ないこと

---

# 5. ワークフロー詳細

## Schedule Trigger

毎週土曜日20:00にワークフローを開始する。

---

## RSS Read

各RSSから最新記事を取得する。

各RSSは独立したNodeとして管理する。

---

## Merge

すべてのRSS記事を1つのデータフローへ統合する。

---

## Aggregate

取得した記事を配列としてまとめる。

出力例

```json
{
  "data": [
    {
      "title": "...",
      "link": "...",
      "pubDate": "..."
    }
  ]
}
```

---

## Edit Fields

OpenAIが扱いやすいように、

```text
articles = data
```

へ変換する。

---

## OpenAI

OpenAIは単なる要約ではなく、

* 記事の重要度判定
* 類似記事の統合
* 初心者向け解説
* 実務での利用例
* 学習優先度の判断

を担当する。

---

## Code Node

OpenAIが生成したMarkdownをNotion保存用に整形する。

### 入力

OpenAIが生成したMarkdown

### 処理

約1800文字単位で分割する。

優先順位

```text
改行
 ↓
空白
 ↓
1800文字で強制分割
```

### 出力

* reportText
* chunkCount
* chunk1
* chunk2
* chunk3
* chunk4
* chunk5
* chunk6

---

## Notion

完成したレポートを保存する。

### Database

Backend Weekly Reports

### 保存項目

| Property    | 内容                               |
| ----------- | -------------------------------- |
| Title       | Backend Weekly Report yyyy-MM-dd |
| Date        | レポート作成日                          |
| Report Type | Industry                         |
| Priority    | High                             |
| Tags        | laravel                          |

本文は Paragraph Block として保存する。

---

# 6. データフロー

```text
RSS取得
    │
    ▼
Merge
    │
    ▼
Aggregate
    │
    ▼
articles
    │
    ▼
OpenAI
    │
    ▼
Markdown
    │
    ▼
Code Node
    │
    ▼
chunk1～chunk6
    │
    ▼
Notion
```

---

# 7. 設計方針

本プロジェクトでは以下の方針を採用する。

## 小さく実装する

RSSは一度に追加せず、

1件追加

↓

テスト

↓

問題なければ次へ

という流れで開発する。

---

## 品質を優先する

情報量よりも、

**「本当に価値のある情報を届けること」**

を重視する。

そのため、情報源は厳選して採用している。

---

## OpenAIを技術レビュアーとして利用する

OpenAIは要約ツールではなく、

技術情報を評価し、

初心者が学ぶべき内容を判断する役割を担う。

---

# 8. バージョン

現在のVersion

```text
v1.0.0
```

ステータス

```text
Released
```
