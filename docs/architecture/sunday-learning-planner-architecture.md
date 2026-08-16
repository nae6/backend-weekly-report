# Sunday Learning Planner Architecture

## Overview

Sunday Learning Planner は、毎週土曜日に生成された Backend Weekly Industry Report をもとに、Current Sprint を最優先として次の1週間の学習計画を生成するワークフローです。

本ワークフローは、ニュースを要約することを目的とせず、

- Current Sprint を前進させる
- 今週学ぶべきことを決定する
- 学習範囲を適切に制御する

ことを目的としています。

生成された Sunday Learning Report は、次の1週間の学習指針として利用されます。

---

# Architecture

```text
                 Backend Weekly
                 Industry Report
                         │
                         ▼
               Get Current Sprint
                         │
                         ▼
              Current Sprint Ready?
                         │
        ┌────────────────┴───────────────┐
        ▼                                ▼
Current Sprintあり                Current Sprintなし
        │                                │
        └────────────────┬───────────────┘
                         ▼
                 Get AI Prompt
                         │
                         ▼
          Prepare OpenAI Request
                         │
                         ▼
         Generate Sunday Learning Report
                         │
                         ▼
          Convert to Notion Properties
                         │
                         ▼
                     Notion
```

---

# Responsibilities

本ワークフローの責務

- Current Sprint取得
- Current Focus取得
- Learner Profile取得
- Current Skills取得
- Backend Weekly Industry Report分析
- 学習テーマ選定
- 学習計画作成
- Sunday Learning Report生成
- Notion保存

対象外

- RSS取得
- 技術ニュース分析
- Backend Weekly Industry Report生成
- 長期ロードマップ作成

---

# Inputs

## Backend Weekly Industry Report

土曜日に生成されたIndustry Report。

今週の技術動向を把握するために利用する。

---

## Current Sprint

現在進行中の学習スプリント。

最優先入力。

---

## Current Focus

Current Sprintの中で現在取り組んでいるテーマ。

---

## Learner Profile

学習目標

現在のレベル

強化したい領域

---

## Current Skills

現在利用できる技術スタック。

AIが学習難易度を判断するために利用する。

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

## 1. Load Learning Context

以下を取得する。

- Current Sprint
- Current Focus
- Learner Profile
- Current Skills

---

## 2. Validate Sprint

Current Sprintが存在するか確認する。

存在しない場合でも処理は継続する。

---

## 3. Prepare AI Request

Backend Weekly Industry ReportとLearning Contextを統合し、

OpenAIへ送信するリクエストを生成する。

---

## 4. AI Analysis

AIは以下を判断する。

- 今週学ぶテーマ
- 学ぶ理由
- 学ばない理由
- 学習STEP
- 完了条件
- 次週候補

---

## 5. Report Generation

Sunday Learning Reportを生成する。

---

## 6. Save

Notionへ保存する。

---

# Outputs

生成される成果物

Sunday Learning Report

内容

- 今週参考にする実践例
- 今週の技術トレンド
- 今週学ぶ理由
- 来週の学習計画
- 今週やらないこと
- 今週説明できるようになりたいこと
- 次週へ引き継ぐ候補
- Mentor's Advice

---

# Learning Decision Flow

AIは以下の順番で判断する。

```text
Current Sprint
        │
        ▼
Current Focus
        │
        ▼
Learner Profile
        │
        ▼
Current Skills
        │
        ▼
Backend Weekly Industry Report
        │
        ▼
Learning Theme
        │
        ▼
Learning Plan
```

Current SprintとIndustry Reportが矛盾する場合は、

Current Sprintを優先する。

---

# AI Prompt Strategy

PromptはWorkflowへ直接保持しない。

すべてNotion AI Prompts Databaseから取得する。

Workflowが参照する情報

- Report Type
- Version
- Active

取得後、

System Prompt

-

User Prompt Template

を組み立ててOpenAIへ送信する。

---

# Design Principles

## Sprint First

Current Sprintを最優先とする。

---

## Learning Before Trend

流行より基礎を優先する。

---

## Small Weekly Goal

学習テーマは原則1件。

1週間で達成できる粒度とする。

---

## Evidence Based

判断理由を必ず説明する。

---

## Separation of Concerns

Saturday

↓

Industry Analysis

Sunday

↓

Learning Planning

---

# Future Expansion

将来的に追加予定

- Learning Backlog
- Monthly Learning Review
- Sprint Retrospective
- Portfolio Review

現在のワークフローへ影響を与えない構造とする。

---

# Version

Current Version

v1.1

Status

Implementation Complete
