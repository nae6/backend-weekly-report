# Backend Report Project Workflow Design

## Overview

本プロジェクトは、毎週土曜日に業界レポートを生成し、その結果を利用して毎週日曜日に学習計画を生成する二段階構成のワークフローです。

役割を分離することで、

- 土曜日は「業界を知る」
- 日曜日は「何を学ぶか決める」

という責務を明確にしています。

---

# Overall Architecture

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

---

# Saturday Workflow

## Purpose

今週のバックエンド業界の変化を整理する。

学習計画は作成しない。

あわせて、Learning Checklistの進捗（Step 0）とGitHub上の実際の活動（Step 0.5）をLearner Profileへ反映する。

---

## Workflow

```text
Claude Scheduled Task (20:00 JST 毎週土曜)

↓

Step 0: Learning Progress Sync
（未反映の完了項目をLearner Profile / Current Sprintへ反映）

↓

Step 0.5: GitHub Activity Sync
（直近7日のコミットをLearner Profile / Current Sprintへ反映）

↓

Duplicate Report Check（同日の重複防止）

↓

WebSearch 5 Official Sources（並列・ドメイン制限）

↓

Remove Duplicate Topics

↓

Claude writes the Report（System Promptはトリガーに埋め込み済み）

↓

Assign Priority + Tags（既存Tags選択肢のみ使用）+ Role Model / Deep Dive Topic

↓

Save to Notion
```

---

## Input

- Web Search Results（5情報源）
- Report Date
- System Prompt（トリガー定義に埋め込み、Notion取得なし）
- Learning Checklist（Step 0の入力）
- GitHub Commits（Step 0.5の入力、直近7日・実行環境で許可されたリポジトリのみ）

---

## Output

Backend Weekly Industry Report

構成

- ① 今週最重要ニュース（基準3件、最大6件）
- ② 今週の技術トレンド
- ③ 関連技術
- ④ Role Model
- ⑤ Deep Dive Topic
- ⑥ 今週の総括

④・⑤は該当なしの週は無理に選ばず「今週は該当なし」とする。

保存先

Notion

---

# Sunday Workflow

## Purpose

Backend Weekly Industry Report を分析し、

Current Sprint を前進させる

次の1週間の学習計画を作成する。

---

## Workflow

```text
Claude Scheduled Task (12:00 JST 毎週日曜)

↓

Get Backend Weekly Industry Report

↓

Get Current Sprint

↓

Current Sprint Ready?

      ├───────────────┐
      │               │
      ▼               ▼

Current Sprintあり    Current Sprintなし

      │               │

      └──────┬────────┘
             ▼

Claude writes the Sunday Learning Report
（System PromptはTrigger定義に直接埋め込み、外部AI APIなし）

↓

Convert to Notion Properties

↓

Save to Notion

↓

Extract 3–6 Action Items

↓

Create Learning Checklist Rows
```

---

# AI Prompt Flow

v2.0.0ではSaturdayとSundayで方針が分かれていたが（Saturdayのみ埋め込み方式）、v2.1.0で統一した。

```text
Workflow (Saturday / Sunday)

↓

System Prompt（それぞれのTrigger定義に直接埋め込み）
```

Notionの「AI Prompts Database」からの動的取得は、Saturdayはv2.0.0、Sundayはv2.1.0で廃止した（無人実行の可用性を優先し、Notionへの問い合わせ回数も削減する）。

このデータベースはv2.1.0以降どちらのWorkflowからも参照されない。

---

# Learning Context Flow

Sunday Workflowでは

Current Sprintだけで判断しない。

```text
Current Sprint

↓

Current Focus

↓

Learner Profile

↓

Current Skills

↓

Backend Weekly Industry Report

↓

Learning Decision
```

---

# Report Flow

```text
Industry Report

↓

Learning Decision

↓

Sunday Learning Report

↓

Learning Checklist（3〜6項目）

↓

（学習者が週内にチェック）

↓

Saturday Step 0 で Learner Profile へ反映
```

Industry Reportは

Learning Plannerの入力であり、

Learning Reportが業界レポートを置き換えるものではない。

Learning Checklistは、計画（Learning Report）と実績（実際に完了したこと）をつなぐ役割を持つ。

---

# Responsibility Separation

## Saturday

担当

- 記事収集
- 技術分析
- 技術トレンド整理
- 実務解説
- Role Model / Deep Dive Topic の紹介

作らないもの

- 学習計画
- Current Sprint評価
- 学習TODO

---

## Sunday

担当

- Current Sprint分析
- 学習テーマ選定
- 学習計画作成
- 学習優先順位判断

作らないもの

- RSS取得
- ニュース分析
- Industry Report生成

---

# Prompt Management

SaturdayもSundayも、Prompt本文はそれぞれのTrigger定義に直接埋め込む（Saturdayはv2.0.0、Sundayはv2.1.0で変更）。

Notion AI Prompts Databaseはv2.1.0以降、どちらのWorkflowからも参照されない。理由は無人実行の可用性を優先し、Notionへの問い合わせ回数を減らすため。

---

# Design Principles

## Single Responsibility

Saturday

↓

Industry Analysis

Sunday

↓

Learning Planning

---

## Reusability

Prompt

Workflow

Notion Database

を独立させる。

---

## Maintainability

Prompt変更時は

Workflowを変更しない。

Workflow変更時は

Promptを変更しない。

---

## Extensibility

将来的に

- Monthly Review
- Sprint Review
- Portfolio Review

などを追加しても、

Saturday Workflow

Sunday Workflow

へ影響を与えない構造とする。

---

## Unattended Safety

すべてのWorkflowは無人・自動実行される前提で設計する。

- Notionスキーマ変更（列・選択肢の追加）を通常運用フローで行わない
- RSS/AtomフィードへのWebFetch直接アクセスを行わない（WebSearchで代替）

いずれも失敗時にエラー終了ではなく承認待ちで無期限にハングするため、

通常のエラーハンドリングでは救えない。設計段階で回避する。

---

# Version

Current Version

v2.3.0

Status

Design Complete（Step 0.5 GitHub Activity Sync、Role Model / Deep Dive Topic を反映）
