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
      Claude writes the Sunday Learning Report
   (System Prompt embedded in the trigger,
           no external AI API)
                         │
                         ▼
          Convert to Notion Properties
                         │
                         ▼
                     Notion
                         │
                         ▼
        Extract 3–6 action items
                         │
                         ▼
           Create Learning Checklist rows
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
- Learning Checklist生成（3〜6項目）
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

## System Prompt

トリガー定義に直接埋め込まれている（v2.1.0以降。それ以前はNotion AI Prompts Databaseから動的取得していた）。

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

## 3. Prepare Request

Backend Weekly Industry ReportとLearning Contextを統合し、

Claude自身への入力として組み立てる。外部AI APIは使用しない。

---

## 4. AI Analysis

Claudeは以下を判断する。

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

## 7. Learning Checklist Generation

生成したSunday Learning Reportから、

具体的で「終わった/終わっていない」を判定できる粒度の実践項目を

3〜6件抽出し、Learning Checklistデータベースへ新規行として保存する。

各項目の初期状態は Done = false, Reflected = false。

このステップで問題が起きても致命的エラーとせず、処理を継続する。

---

# Outputs

生成される成果物

## Sunday Learning Report

- 今週参考にする実践例
- 今週の技術トレンド
- 今週学ぶ理由
- 来週の学習計画
- 今週やらないこと
- 今週説明できるようになりたいこと
- 次週へ引き継ぐ候補
- Mentor's Advice

## Learning Checklist（新規, v2.0.0）

- Name（実践項目）
- Done（学習者が手動でチェック）
- Reflected（Saturday Step 0が反映済みか）
- Source Report Date

Saturday Step 0（Learning Progress Sync）が読み取り、

Learner Profile / Current Sprintへ反映する。

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

v2.0.0まではNotion AI Prompts Databaseから動的取得していたが、v2.1.0でSaturdayと同じくトリガー定義への直接埋め込みへ変更した。

理由

AI Prompts Databaseには実質Sunday用の1行しか存在せず、「1行しかない可変データのために毎回Notionへ問い合わせる」というコストに見合う価値がなかったため。埋め込み方式へ統一することで

- 実行のたびのNotion問い合わせが2回減る(検索1回・取得1回)
- Report Type / Version / Activeの設定ミスによる不動作の余地がなくなる
- SaturdayとSundayでプロンプトの持ち方が揃い、認知負荷が下がる

というメリットがある。AI Prompts Databaseはv2.1.0以降どちらのWorkflowからも参照されない（Notion側に残すかどうかは運用者の判断に委ねる）。

Prompt変更時は、トリガー定義そのものを更新する。

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

- 重複防止チェック（Backend Weekly Report同様の仕組み）
- Learning Backlog
- Monthly Learning Review
- Sprint Retrospective
- Portfolio Review

現在のワークフローへ影響を与えない構造とする。

---

# Version

Current Version

v2.1.0

Status

Implementation Complete（Prompt埋め込み方式へ統一、AI Prompts Database依存を解消）
