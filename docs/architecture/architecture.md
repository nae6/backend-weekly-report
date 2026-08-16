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
                  RSS / Official Sources
                           │
                           ▼
             Saturday Backend Weekly Report
                           │
                           ▼
                    Notion Database
                           │
          ┌────────────────┴────────────────┐
          │                                 │
          ▼                                 ▼
 AI Prompts Database              Current Sprint Database
          │                                 │
          └────────────────┬────────────────┘
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

## AI Prompts Database

役割

AI Promptを一元管理する。

保持する情報

- Report Type
- Version
- Active
- System Prompt
- User Prompt Template

WorkflowへPrompt本文は保持しない。

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

# Prompt Architecture

PromptはWorkflowから分離する。

```text
Workflow

↓

AI Prompts Database

↓

System Prompt

+

User Prompt Template

↓

OpenAI
```

Prompt変更時はWorkflowを変更しない。

---

# Workflow Architecture

## Saturday

```text
Collect Articles

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

v1.1

Status

Architecture Complete
