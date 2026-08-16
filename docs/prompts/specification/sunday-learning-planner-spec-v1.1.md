# Sunday Learning Planner Specification v1.1

## Overview

Sunday Learning Planner は、毎週土曜日に生成された Backend Weekly Industry Report をもとに、Current Sprint を最優先として次の1週間の学習計画を生成するワークフローです。

本ワークフローの目的はニュースを要約することではありません。

Current Sprint を継続しながら、学習者が無理なく一歩成長できる学習テーマを決定し、実践可能な学習計画へ変換することを目的とします。

---

# Objectives

本ワークフローは以下を目的とします。

- Current Sprintを継続的に前進させる
- 今週の技術動向を学習へ結び付ける
- 学習範囲を広げすぎない
- 実務につながるアウトプットを重視する
- 1週間で達成可能な学習計画を作成する
- 学習テーマを明確な根拠をもって選定する

---

# Scope

対象

- Backend Weekly Industry Report の分析
- Current Sprint の分析
- Current Focus の分析
- Learner Profile の分析
- Current Skills の分析
- 学習テーマの選定
- 学習計画の作成
- 次週への引き継ぎ候補の提示

対象外

- RSS収集
- ニュース検索
- Industry Reportの生成
- 長期ロードマップ作成
- 月単位以上の学習計画

---

# Inputs

## Primary Input

### Backend Weekly Industry Report

土曜日に生成されたレポート。

今週の技術動向を把握するための入力として利用する。

---

## Learning Context

### Current Sprint

現在進行中の学習スプリント。

最優先入力。

---

### Current Focus

Current Sprint の中で現在取り組んでいるテーマ。

---

### Learner Profile

現在の目標

現在の学習レベル

今後強化したい領域

---

### Current Skills

現在利用できる技術スタック。

AIが過大・過小評価しないために利用する。

---

## Prompt

Notion AI Prompts Database に保存された

- System Prompt
- User Prompt Template

WorkflowへPrompt本文を直接保持しない。

---

# Decision Priority

AIは以下の順番で判断する。

1. Current Sprint
2. Current Focus
3. Learner Profile
4. Current Skills
5. Backend Weekly Industry Report
6. 将来重要になる技術

Industry Reportは重要な入力だが、

Current Sprintと矛盾する場合はCurrent Sprintを優先する。

流行より基礎を優先する。

---

# Learning Theme Rules

学習テーマは原則1件。

必要に応じて補助テーマを追加してもよいが、

メインテーマは1件のみとする。

テーマは以下を満たす。

- 1週間で完了できる
- Current Sprintへ直接貢献する
- 実務につながる
- ポートフォリオへ反映できる
- 学習成果を確認できる

---

# Learning Goal Rules

学習ゴールは

達成を確認できる形

で設定する。

例

- 実装できる
- 修正できる
- 説明できる
- テストできる
- 比較できる
- 設計理由を説明できる

---

# Learning Plan Rules

学習計画は

ゴール

↓

必要な知識

↓

小さな実践

↓

確認

↓

自分の言葉で説明

の流れで構成する。

曜日単位ではなく

STEP形式で出力する。

---

# Reference Rules

参考資料は以下を優先する。

1. 公式ドキュメント
2. 公式チュートリアル
3. OSS
4. OSS Maintainer
5. 企業Tech Blog
6. Backend Weekly Industry Report

知名度ではなく

- 信頼性
- Current Sprintとの関連
- 再現性

を重視する。

---

# Output

生成するレポート

# Sunday Learning Report

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

# AI Responsibilities

AIは以下を行う。

- 学習テーマの選定
- 優先順位の判断
- 学習計画の作成
- 判断理由の説明
- 学ばないテーマの提示
- 次週候補の提示

ニュース要約だけを目的としてはいけない。

---

# Workflow

Schedule Trigger

↓

Get Backend Weekly Industry Report

↓

Get Current Sprint

↓

Current Sprint Ready?

↓

Get AI Prompt

↓

Prepare OpenAI Request

↓

Generate Sunday Learning Report

↓

Convert to Notion Properties

↓

Save to Notion

---

# Output Quality

出力は以下を満たすこと。

- Markdown形式
- 初学者にも理解できる
- Current Sprintを最優先する
- 学習テーマは原則1件
- 実務との関係を説明する
- 判断理由を説明する
- 1週間で実行可能
- 学習成果を確認できる
- ニュース要約だけで終わらない

---

# Non Goals

本ワークフローでは以下は行わない。

- Current Sprintを無視する
- 流行だけで学習テーマを変更する
- 複数の大規模テーマを同時提案する
- 1週間で終わらない計画を作る
- Backend Weekly Industry Reportにない事実を追加する

---

# Relationship with Backend Weekly Industry Report

Saturday

Collect Articles

↓

Backend Weekly Industry Report

↓

Notion

↓

Sunday Learning Planner

↓

Sunday Learning Report

↓

Notion

---

# Version

Current Version

v1.1

Status

Design Complete

Implementation Ready
