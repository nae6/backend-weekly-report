# Backend Weekly Report 設計書 Version 1.0（完成版）

**Project Name**：Backend Weekly Report
**Version**：1.0.0
**Status**：Released
**作成日**：2026-08-15

---

# 1. プロジェクト概要

## 目的

バックエンドエンジニアを目指す初学者が、毎週の技術情報を効率よく収集・整理し、学習すべき内容を判断できるようにする。

n8nをワークフロー基盤として利用し、

* 公式情報
* OSS
* 企業Tech Blog

から最新記事を取得し、OpenAIで重要度を判断したうえで、初心者向けの週次レポートを生成し、Notionへ自動保存する。

---

# 2. Version 1.0 の完成目標

毎週土曜日20:00に自動で以下を実行する。

```text
技術情報取得
        ↓
記事統合
        ↓
AIによる重要度判定
        ↓
初心者向けレポート生成
        ↓
Notionへ保存
```

Version1.0では**土曜日版（Backend Weekly Report）**を完成対象とする。

---

# 3. システム構成

| サービス       | 用途          |
| ---------- | ----------- |
| n8n        | ワークフロー実行    |
| RSS        | 最新記事取得      |
| OpenAI API | 記事分析・レポート生成 |
| Notion     | レポート保存      |

---

# 4. ワークフロー

```text
Schedule Trigger
        │
        ▼
RSS - PHP Official
RSS - Laravel News
RSS - Symfony Blog
RSS - AWS What's New
RSS - Docker Blog
RSS - OpenAI Developer Blog
RSS - Money Forward Developers
RSS - LY Tech Blog
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

# 5. RSS一覧（Version 1.0）

## 公式・OSS

| RSS                   | 用途                  |
| --------------------- | ------------------- |
| PHP Official          | PHP本体               |
| Laravel News          | Laravel全般           |
| Symfony Blog          | Symfony・Laravel内部技術 |
| AWS What's New        | AWS最新情報             |
| Docker Blog           | Docker関連            |
| OpenAI Developer Blog | OpenAI API・AI技術     |

## 企業Tech Blog

| RSS                      | 用途        |
| ------------------------ | --------- |
| Money Forward Developers | 実務・設計・運用  |
| LY Tech Blog             | 大規模サービス開発 |

---

# 6. RSS選定方針

採用基準

* 公式情報を優先
* 実務で利用頻度が高い技術
* 初学者が学ぶ価値が高い情報
* 重複が少ない情報源

採用しなかった情報源

| RSS                | 理由                              |
| ------------------ | ------------------------------- |
| GitHub Releases    | Laravel News・Symfony Blogと重複が多い |
| Google Developers  | 対象範囲が広すぎる                       |
| Microsoft DevBlogs | .NET中心                          |
| Vercel Blog        | フロントエンド寄り                       |
| InfoQ              | 記事数が多くノイズになりやすい                 |

---

# 7. OpenAIの役割

OpenAIは単なる要約ではなく、

* 記事の重要度判定
* 類似記事の統合
* 初心者向け解説
* 実務での利用例
* 学習優先度の判断

を担当する。

---

# 8. レポート構成

毎週生成されるレポートは以下で統一する。

```text
Backend Weekly Report

① 今週最重要ニュース（最大3件）

② 今週の技術トレンド

③ Mermaid図（必要時のみ）

④ 今週の学習TODO（最大3件）

⑤ 一言アドバイス
```

文字数

* 約800〜1200文字
* 約3分で読了

---

# 9. Code Node設計

## 入力

OpenAIが生成したMarkdown

```
output[0].content[0].text
```

---

## 処理

最大1800文字単位で分割する。

優先順位

```text
改行
 ↓
空白
 ↓
1800文字で強制分割
```

---

## 出力

```text
reportText

chunkCount

chunk1
chunk2
chunk3
chunk4
chunk5
chunk6
```

---

# 10. Notion設計

Database

```
Backend Weekly Reports
```

## Title

```
Backend Weekly Report yyyy-MM-dd
```

例

```
Backend Weekly Report 2026-08-15
```

---

## Properties

| Property    | 内容       |
| ----------- | -------- |
| Date        | レポート作成日  |
| Report Type | Industry |
| Priority    | High     |
| Tags        | laravel  |

---

## Page Content

Paragraph Block

```
chunk1
chunk2
chunk3
chunk4
chunk5
chunk6
```

---

# 11. データフロー

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
Code
     │
     ▼
chunk1～chunk6
     │
     ▼
Notion
```

---

# 12. テスト結果

## RSS

* PHP Official
* Laravel News
* Symfony Blog
* AWS What's New
* Docker Blog
* OpenAI Developer Blog
* Money Forward Developers
* LY Tech Blog

すべて正常取得

---

## Workflow

確認項目

* RSS取得
* Merge
* Aggregate
* Edit Fields
* OpenAI
* Code
* Notion保存

結果

**すべて正常終了**

---

## E2Eテスト

実際にワークフロー全体を実行し、

* レポート生成
* Notion保存
* 出力内容

を確認。

結果

**正常終了**

---

# 13. ディレクトリ構成

```text
backend-weekly-report/

├── README.md

├── docs/
│   ├── backend-weekly-report-design-v1.0.md
│   ├── architecture.md
│   └── version-history.md

└── workflows/
    └── backend-weekly-report-v1.0.0.json
```

---

# 14. Version 1.0 完了条件

以下をすべて満たした。

* RSS取得
* Merge
* Aggregate
* OpenAI
* Code
* Notion保存
* E2Eテスト
* レポート品質確認

**Version 1.0 完了**

---

# 15. Version管理

| Version | 内容                           |
| ------- | ---------------------------- |
| v1.0.0  | Backend Weekly Report 初回リリース |
| v1.1.0  | Role Model Analyzer（日曜日版）予定  |

---

# 16. Version 1.1 予定

Version1.0で生成したレポートを利用し、

* ロールモデル分析
* 技術トレンド分析
* 学習計画作成

を自動化する。

新しいワークフローとして実装する。

---

# 17. 今後の開発方針

今後は以下の開発サイクルを採用する。

```text
要件定義
    ↓
設計
    ↓
小さく実装
    ↓
単体テスト
    ↓
結合テスト
    ↓
E2Eテスト
    ↓
GitHub Release
```

新機能はVersion単位で管理し、完成版ごとにGitタグを作成する。

---

# Version 1.0 リリース

**Backend Weekly Report Version 1.0.0**

Status：**Released**
