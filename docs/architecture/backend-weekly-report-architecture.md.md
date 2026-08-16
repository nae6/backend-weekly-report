# Backend Weekly Industry Report Architecture

## Overview

Backend Weekly Industry Report は、毎週土曜日にバックエンド業界の最新情報を収集・分析し、今週の技術動向を整理するワークフローです。

このワークフローは、ニュースを一覧化することではなく、

- 今週何が起きたのか
- なぜ重要なのか
- 実務へどのような影響があるのか

を整理した Industry Report を生成することを目的としています。

生成されたレポートは Sunday Learning Planner の入力として利用されます。

---

# Architecture

```text
                 RSS / Official Sources
                          │
                          ▼
                 Collect Articles
                          │
                          ▼
                 Normalize Articles
                          │
                          ▼
                Remove Duplicates
                          │
                          ▼
                Get AI Prompt
                          │
                          ▼
          Prepare OpenAI Request
                          │
                          ▼
      Backend Weekly Industry Report
                          │
                          ▼
                 Convert Properties
                          │
                          ▼
                     Notion
```

---

# Responsibilities

本ワークフローの責務

- RSS記事収集
- Backend関連記事抽出
- 重複除去
- 技術分析
- 技術トレンド整理
- 実務への影響整理
- Industry Report生成
- Notion保存

対象外

- Current Sprint分析
- 学習計画
- 学習テーマ決定
- TODO生成

---

# Inputs

## RSS Articles

記事一覧

含まれる情報

- Title
- URL
- Published Date
- Content

---

## AI Prompt

Notion AI Prompts Database

取得項目

- Report Type
- Version
- Active
- System Prompt
- User Prompt Template

WorkflowへPrompt本文は保持しない。

---

# Processing

## 1. Collect Articles

RSSから記事取得

---

## 2. Normalize

OpenAIへ渡しやすい形式へ変換

---

## 3. Remove Duplicate Topics

同一テーマの記事を統合する。

---

## 4. AI Analysis

記事を分析し

- 技術的重要性
- 実務への影響
- 初学者向け解説

を生成する。

---

## 5. Report Generation

Backend Weekly Industry Report を生成する。

---

## 6. Save

Notionへ保存する。

---

# Outputs

生成される成果物

Backend Weekly Industry Report

内容

- 今週最重要ニュース
- 技術トレンド
- 関連技術
- Mermaid図（必要時）
- 今週の総括

---

# Consumers

生成された Industry Report は

Sunday Learning Planner

のみが利用する。

```text
Backend Weekly Industry Report

        │

        ▼

Sunday Learning Planner

        │

        ▼

Sunday Learning Report
```

---

# AI Prompt Strategy

Promptは

Notion AI Prompts Database

から取得する。

Workflowには

Prompt本文を書かない。

取得条件

- Report Type
- Version
- Active

---

# Design Principles

## Separation of Concerns

Industry Report

↓

Learning Report

を完全に分離する。

---

## Single Responsibility

Saturday Workflow は

業界分析だけを担当する。

---

## Source Priority

一次情報を最優先する。

優先順位

1. Official Docs
2. Release Notes
3. GitHub Release
4. Vendor Blog
5. Tech Blog

---

## Future Expansion

将来追加予定

- Monthly Industry Report
- Quarterly Technology Review
- Annual Trend Report

現在のWorkflowへ影響を与えない構造とする。

---

# Version

Current Version

v1.0

Status

Implementation Complete
