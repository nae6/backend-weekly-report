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
                RSS / Official Sources
                         │
                         ▼
        Saturday Backend Weekly Report
                         │
                         ▼
                 Notion Database
                         │
                         ▼
          Sunday Learning Planner
                         │
                         ▼
           Sunday Learning Report
                         │
                         ▼
                 Notion Database
```

---

# Saturday Workflow

## Purpose

今週のバックエンド業界の変化を整理する。

学習計画は作成しない。

---

## Workflow

```text
Schedule Trigger

↓

Collect RSS Articles

↓

Normalize Articles

↓

Remove Duplicate Topics

↓

Get AI Prompt

↓

Prepare OpenAI Request

↓

Generate Backend Weekly Industry Report

↓

Convert to Notion Properties

↓

Save to Notion
```

---

## Input

- RSS Articles
- Report Date
- Backend Weekly Industry Report Prompt

---

## Output

Backend Weekly Industry Report

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
Schedule Trigger

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

Get AI Prompt

↓

Prepare OpenAI Request

↓

Generate Sunday Learning Report

↓

Convert to Notion Properties

↓

Save to Notion
```

---

# AI Prompt Flow

PromptはWorkflowへ直接保持しない。

```text
Workflow

↓

Notion AI Prompts Database

↓

Report Type

↓

Version

↓

Active

↓

System Prompt

+

User Prompt Template
```

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
```

Industry Reportは

Learning Plannerの入力であり、

Learning Reportが業界レポートを置き換えるものではない。

---

# Responsibility Separation

## Saturday

担当

- 記事収集
- 技術分析
- 技術トレンド整理
- 実務解説

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

Promptは

Notion AI Prompts Database

で管理する。

WorkflowへPrompt本文を書かない。

Workflowが取得する情報

- Report Type
- Version
- Active

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

# Version

Current Version

v1.1

Status

Design Complete
