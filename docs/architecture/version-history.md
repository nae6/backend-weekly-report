# Version History

本ドキュメントは Backend Report Project の設計・ワークフロー・Prompt・Architecture の変更履歴を管理します。

---

# v1.1

## Release Date

2026-08

---

## Overview

Backend Report Project を

「業界分析」

と

「学習計画」

へ完全に分離しました。

また、Prompt・Workflow・Architecture を独立管理できる構成へ変更しました。

---

# Added

## Sunday Learning Planner

新規追加

目的

Backend Weekly Industry Report を分析し、

Current Sprint を前進させる

1週間の学習計画を生成する。

---

## Sunday Learning Report

新規追加

生成内容

- 今週参考にする実践例
- 今週の技術トレンド
- 今週学ぶ理由
- 来週の学習計画
- 今週やらないこと
- 今週説明できるようになりたいこと
- 次週へ引き継ぐ候補
- Mentor's Advice

---

## AI Prompts Database

PromptをWorkflowから分離。

Notion Databaseで管理する方式へ変更。

追加項目

- Report Type
- Version
- Active
- System Prompt
- User Prompt Template

---

## Current Sprint

Sunday Workflowの最重要入力として追加。

Current Sprintを中心に

Learning Themeを決定する方式へ変更。

---

## Current Focus

Current Sprint内で

現在取り組んでいるテーマを追加。

---

## Current Skills

学習レベル判定用入力として追加。

---

## Learner Profile

固定Promptではなく

入力データとして扱う構成へ変更。

---

# Changed

## Saturday Workflow

役割を

Industry Analysis

へ限定。

削除

- 学習TODO
- 学習計画
- 学習アドバイス

追加

- 技術トレンド
- 関連技術
- Industry Summary

---

## Sunday Workflow

ニュース要約から

Learning Planning

へ変更。

追加

- Current Sprint分析
- 学習テーマ選定
- 学習STEP
- 完了条件
- 学ばないテーマ
- 次週候補

---

## Prompt Management

WorkflowへPrompt本文を書かない構成へ変更。

Promptは

Notion AI Prompts Database

から取得する。

---

## Architecture

土曜日

↓

Industry Analysis

日曜日

↓

Learning Planning

の二段構成へ変更。

---

# Improved

## Learning Theme Selection

学習テーマ選定ルールを追加。

優先順位

1. Current Sprint
2. Current Focus
3. Learner Profile
4. Current Skills
5. Backend Weekly Industry Report

---

## Goal Definition

学習ゴールを

説明できる

実装できる

修正できる

など

達成確認可能な形式へ変更。

---

## Learning Plan

曜日単位ではなく

STEP形式へ変更。

---

## Reference Selection

参考資料優先順位を追加。

1. Official Docs
2. Official Tutorial
3. OSS
4. OSS Maintainer
5. Tech Blog

---

## Workflow Design

Workflow設計書を追加。

土曜日・日曜日の責務を明確化。

---

# Directory Structure

追加

```text
docs/
├── architecture/
├── prompts/
│   ├── implementations/
│   ├── specification/
│   └── workflow/
└── workflows/
```

---

# Breaking Changes

なし。

既存のBackend Weekly Industry Reportは

そのまま利用可能。

Sunday Learning Plannerのみ追加。

---

# Future Roadmap

予定

- Learning Backlog
- Monthly Learning Review
- Sprint Review
- Portfolio Review
- Career Review

現在のArchitectureを維持したまま追加できる構成とする。

---

# Current Status

Architecture

Completed

Prompt

Completed

Specification

Completed

Workflow

Implemented

Prompt Management

Implemented

Sunday Learning Planner

Implemented

Backend Weekly Industry Report

Implemented

---

# Current Version

v1.1

Status

Stable
