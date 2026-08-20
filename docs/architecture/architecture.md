# Backend Report Project Architecture

## Overview

Backend Report Project は、バックエンドエンジニアを目指す学習者向けに、毎週の技術情報収集から学習計画作成までを自動化するプロジェクトです。

本プロジェクトは

- 業界分析
- 学習計画
- Prompt管理
- Workflow管理

を明確に分離し、それぞれを独立して保守・改善できる構成を採用しています。

---

# Objectives

本プロジェクトの目的は以下です。

- バックエンド業界の最新動向を継続的に把握する
- 学習者のCurrent Sprintを前進させる
- 学習範囲を適切に制御する
- 実務へつながる学習を支援する
- Prompt・Workflow・Architectureを独立管理する

---

# System Architecture

```text
                  Web Search (5 official sources)
                           │
                           ▼
             Saturday Backend Weekly Report
                (+ Learning Progress Sync)
                           │
                           ▼
                    Notion Database
                           │
                           ▼
                 Current Sprint Database
                           │
                           ▼
               Sunday Learning Planner
                           │
                           ▼
     Sunday Learning Report + Learning Checklist
                           │
                           ▼
                    Notion Database
                           │
              (checked off during the week)
                           │
                           ▼
        fed back into next Saturday's Step 0
```

Both Saturday's and Sunday's prompts are embedded directly in their Claude Scheduled Task, not fetched from Notion — see [Changed: Prompt Management](version-history.md) in v2.0.0 (Saturday) and v2.1.0 (Sunday).

---

# Components

## Backend Weekly Industry Report

役割

- RSS記事収集
- 技術分析
- 業界動向整理
- Industry Report生成

生成物

Backend Weekly Industry Report

---

## Sunday Learning Planner

役割

- Current Sprint分析
- 学習テーマ選定
- 学習計画作成
- Sunday Learning Report生成

生成物

Sunday Learning Report

---

## AI Prompts Database（v2.1.0で廃止）

v2.0.0まではSundayのPromptをこのデータベースから動的取得していたが、v2.1.0でSaturdayと同じ埋め込み方式に統一したため、どちらのTriggerからも参照されなくなった。

保持していた情報（参考）

- Report Type
- Version
- Active
- System Prompt
- User Prompt Template

Notion上のデータベース自体は自動では削除されない。残すか削除するかは運用者の判断。

---

## Current Sprint Database

役割

現在の学習状況を管理する。

保持する情報

- Sprint
- Current Focus
- Status
- Goal
- Priority

Sunday Learning Plannerが利用する。

---

## Reports Database

役割

生成されたレポートを保存する。

対象

- Backend Weekly Industry Report
- Sunday Learning Report

---

## Learning Checklist Database

役割

その週の学習計画を、実行可能な単位に分解して管理する。

保持する情報

- Name
- Done
- Reflected
- Source Report Date

Sunday Learning Plannerが生成し、

Saturday Step 0（Learning Progress Sync）が読み取る。

チェック（Done）は学習者本人がNotion上で手動で行う。

---

# Prompt Architecture

v2.0.0ではSaturdayのみPromptをTrigger定義に直接埋め込む例外運用だったが、v2.1.0でSundayも同じ方式に統一した。

```text
Saturday Trigger / Sunday Trigger
（System PromptをそれぞれのTrigger定義に直接埋め込み）

↓

Claude (self)
```

外部AI API（OpenAI）は使用しない。Claude自身がレポートを執筆する。

Prompt本文を直接埋め込んでいる理由は、無人実行における可用性を優先し、Notion側の状態（Active/Versionフラグの設定ミスなど）に依存する箇所を減らすため。あわせて、Notionへの問い合わせ回数そのものを減らせるという副次効果もある。

Prompt変更時はTriggerの動作フロー（Step順序）を変更しない。Prompt本文を編集したい場合は、対応するTrigger定義を直接更新する（Notion側にはもうプロンプトの正はない）。

---

# Workflow Architecture

## Saturday

```text
Step 0: Learning Progress Sync

↓

Duplicate Check

↓

Collect Articles (WebSearch)

↓

Industry Analysis

↓

Industry Report

↓

Notion
```

---

## Sunday

```text
Industry Report

↓

Current Sprint

↓

Learning Decision

↓

Learning Report

↓

Notion

↓

Learning Checklist (3–6 items)
```

---

# Responsibility Separation

## Saturday

担当

- 情報収集
- 技術分析
- 業界整理

担当しない

- 学習テーマ
- 学習計画
- TODO

---

## Sunday

担当

- 学習テーマ
- Current Sprint分析
- 学習計画

担当しない

- RSS取得
- 技術記事分析
- Industry Report生成

---

# Data Flow

```text
RSS

↓

Industry Report

↓

Notion

↓

Learning Planner

↓

Learning Report

↓

Notion
```

---

# Design Principles

## Single Responsibility

各Workflowは1つの責務だけを持つ。

---

## Separation of Concerns

Industry Analysis

Learning Planning

Prompt Management

Workflow

Architecture

を完全に分離する。

---

## Prompt First

PromptはWorkflowへ埋め込まない。

Notion Databaseで管理する。

---

## Sprint First

Current Sprintを最優先する。

---

## Official Source First

技術情報は一次情報を優先する。

---

## Maintainability

Prompt

Workflow

Architecture

を独立して改善できる構成とする。

---

## Extensibility

将来的に追加予定

- Monthly Review
- Sprint Review
- Portfolio Review
- Learning Backlog
- Career Review

既存Workflowへ影響を与えず追加できる構造とする。

---

## Unattended Safety

すべてのTriggerは人が張り付いていない状態で実行される前提とする。

このため、以下は通常運用フローで行わない。

- Notionデータベースのスキーマ変更（列・選択肢の追加）
- RSS/AtomフィードへのWebFetch直接アクセス

これらは失敗時に「エラーで終了する」のではなく

「承認待ちのまま無期限にハングする」ため、通常のエラーハンドリングでは救えない。

スキーマ変更が必要な場合は、対話セッションで人が明示的に承認して行う。

記事収集はドメイン制限付きWebSearchで代替する。

---

# Directory Structure

```text
docs/
├── architecture/
├── prompts/
│   ├── implementations/
│   ├── specification/
│   └── workflow/
│
└── workflows/
```

---

# Version

Current Version

v2.1.0

Status

Architecture Complete
