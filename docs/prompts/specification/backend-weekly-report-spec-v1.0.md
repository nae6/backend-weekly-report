# Backend Weekly Industry Report Specification v1.0

## Overview

Backend Weekly Industry Report は、毎週収集したバックエンド関連の記事を分析し、今週の技術動向を整理するためのレポートを生成するワークフローです。

このレポートはニュースの一覧ではなく、バックエンド業界全体の流れを整理し、日曜日に生成する Learning Report の判断材料となることを目的としています。

---

# Objectives

本ワークフローの目的は以下です。

- 今週のバックエンド業界全体の動向を整理する
- 重要な技術トピックを抽出する
- 実務への影響を分かりやすく説明する
- 初学者でも理解できる技術解説を提供する
- 日曜日の Learning Planner の入力データを作成する

---

# Scope

本ワークフローが対象とする内容

- Backend関連ニュース
- Framework
- Programming Language
- Database
- Infrastructure
- Cloud
- Docker
- Security
- Testing
- Architecture
- AI × Software Engineering
- OSS
- Tech Blog
- Release情報

対象外

- 学習計画の作成
- 学習TODOの提案
- 長期キャリア計画
- Current Sprintの評価

これらは Sunday Learning Planner が担当する。

---

# Inputs

## Primary Input

### Collected Articles

RSS等から収集した記事一覧。

記事には以下の情報を含む。

- タイトル
- 公開日
- URL
- 本文または要約

---

## Report Date

レポート生成日。

---

# AI Responsibilities

AIは以下を実施する。

- 記事を分析する
- 重複するニュースを統合する
- 技術トピックを整理する
- 実務への影響を説明する
- 初学者向け解説を作成する
- 今週の業界全体の流れをまとめる

学習計画は作成しない。

---

# Topic Selection Rules

以下を優先する。

1. Backend技術
2. 公式リリース
3. Framework
4. Database
5. Infrastructure
6. Security
7. Testing
8. Architecture
9. AI × Software Engineering

重要度の低いニュースは掲載しなくてもよい。

---

# Source Priority

以下の順で信頼する。

1. 公式ドキュメント
2. Release Note
3. GitHub Release
4. Vendor Blog
5. Tech Blog

可能な限り一次情報を利用する。

---

# Output

AIは以下を生成する。

# Backend Weekly Industry Report

内容

- 今週最重要ニュース
- 今週の技術トレンド
- 関連技術
- 必要に応じたMermaid図
- 今週の総括

---

# Output Rules

ニュースは同じテーマごとに整理する。

記事単位で出力しない。

以下を必ず含める。

- 何が起きたか
- なぜ重要なのか
- 実務でどう利用されるか
- 初学者向け解説
- 学習優先度
- 公式情報

---

# Diagram Rules

Mermaidは必要な場合のみ出力する。

以下のような場合に利用する。

- システム構成
- データの流れ
- 技術同士の関係
- アーキテクチャ

不要な場合は省略する。

---

# Quality Rules

出力は以下を満たすこと。

- Markdown形式
- 初学者にも理解できる
- 実務との関係を説明する
- 一次情報を優先する
- 同じニュースを統合する
- 推測を書かない
- 技術トレンドを整理する
- Learning Planner の入力として利用できる品質を保つ

---

# Non Goals

本ワークフローでは以下は行わない。

- 学習計画を作成する
- Current Sprintを考慮する
- 学習TODOを提案する
- 長期ロードマップを作成する
- ニュースを時系列順に並べるだけのレポートを作る

---

# Relationship with Sunday Learning Planner

Saturday

Collected Articles

↓

Backend Weekly Industry Report

↓

Notion

↓

Sunday Learning Planner

↓

Backend Career & Learning Report

---

# Version

Current Version

v1.0

Status

Design Complete

Implementation Ready
