# Version History

本ドキュメントは Backend Report Project の設計・ワークフロー・Prompt・Architecture の変更履歴を管理します。

---

# v2.2.0

## Release Date

2026-09

---

## Overview

実行環境を、Claude Code Remoteの共有Cowork環境から、本リポジトリ(backend-weekly-report)専用のClaude Code Remote環境へ移行しました。

Trigger定義(Prompt本文・スケジュール・Notion連携内容)は変更していません。変更したのは「どこで実行されるか」のみです。

---

# Changed

## Execution Environment

両Trigger(Saturday / Sunday)の実行環境を

Cowork Remote環境(汎用・複数プロジェクト共有)

から

本リポジトリ専用のClaude Code Remote環境

へ変更。

理由

- 実行環境をリポジトリのコンテキストに紐付け、将来的にコード(本リポジトリ)を直接読み書きするステップを追加しやすくするため
- Cowork環境は他プロジェクトと共有されており、本プロジェクト専用の設定として分離するため

---

## Notion Connector 付与方式

新環境では、この組織のアカウント設定上、Trigger作成APIから`connectors`パラメータを直接指定できない制約があることが判明した。

そのため、Notion Connectorは claude.ai の Routines/Cowork 管理画面から、Trigger単位で手動で有効化する運用とした。

新規にTriggerを再作成・移行する場合は、この手動ステップが必要になる点に注意。

---

## Old Triggers

移行元(Cowork環境)のTriggerは削除せず、`enabled: false`で無効化のみ行った。ロールバックが必要な場合は再度有効化できる。

---

# Considered / Rejected

## Sunday Learning PlannerへのGitHub活動反映ステップ追加(一時的に追加後、削除)

Saturday Step 0.5(GitHub反映)と同等の処理をSunday側にも追加することを一時的に試みたが、以下の理由で撤回した。

- SaturdayとSundayの実行間隔は約16時間しかなく、どちらも「直近7日以内のコミット」を参照範囲としているため、Sunday側が新たに拾う差分はSaturday実行後〜Sunday実行までの狭い時間帯のみ
- その狭い差分のために、GitHub API呼び出し・LLMによる再要約・Notionへの再書き込みを毎週丸ごと繰り返すのはコストに見合わない
- 「重複記載を避けて統合」をLLM任せで週2回繰り返すことで、Current Skills/Current Focusの文面が徐々にブレる・重複するリスクがある

結論として、GitHub反映(Learning Progress Sync)はSaturdayのみが担当する、というv2.0.0以来の設計を維持する。

---

# Directory Structure

変更なし。

---

# Breaking Changes

なし。Trigger定義(Prompt・スケジュール・Notionスキーマ)は変更していない。実行環境(インフラ)のみの変更。

---

# Future Roadmap

予定(v2.1.0から変更なし)

- Sunday Workflowへの重複防止チェック追加
- Learning Checklist抽出粒度のチューニング
- Learning Backlog
- Monthly Learning Review
- Sprint Retrospective
- Portfolio Review
- Career Review

検討中(未着手)

- Learning Contexts データベースをLearner Profileへ統合し、データベース数をさらに減らす案(Sprintの履歴管理をどう扱うか要検討のため保留)

---

# Current Status

Architecture

Completed

Prompt

Completed(v2.1.0から変更なし)

Specification

Completed

Workflow

Migrated to repository-scoped Claude Code Remote environment

Learning Progress Sync

Implemented(Saturdayのみ。Sundayへの拡張は検討の上見送り)

Learning Checklist

Implemented

---

# Current Version (as of v2.2.0)

v2.2.0

Status

Stable

---

# v2.1.0

## Release Date

2026-08

---

## Overview

Notionの「AI Prompts」データベースを廃止しました。

Sunday Learning PlannerのSystem Prompt/User Prompt Templateを、

Saturdayと同じくトリガー定義への直接埋め込み方式へ変更したことで、

このデータベースはどちらのWorkflowからも参照されなくなりました。

---

# Changed

## Prompt Management (Sunday)

Notion AI Prompts Databaseからの動的取得(Name="Sunday Learning Planner" AND State="Active"を検索)をやめ、

System Prompt / User Prompt Templateの内容をトリガー定義に直接埋め込む方式へ変更。

v2.0.0でSaturdayに適用した変更と同じ理由による(無人実行の可用性優先、Notion側の設定ミスに依存する箇所の削減)。

副次効果として、Sunday実行時のNotion問い合わせが2回(検索1回・取得1回)減った。

---

# Removed

## AI Prompts Database(参照なし)

v2.0.0時点でSaturdayからは既に参照されておらず、v2.1.0でSundayからの参照もなくなったため、

このデータベースは実質的に不要になった。

Notion上のデータベース自体は自動では削除しない。残すか削除するかは運用者の判断に委ねる。

---

# Directory Structure

変更なし。

---

# Breaking Changes

なし。Notion側のプロパティ・データは変更していない(AI Prompts Databaseへの書き込み・読み込みを単に行わなくなっただけ)。

---

# Future Roadmap

予定(v2.0.0から変更なし)

- Sunday Workflowへの重複防止チェック追加
- Learning Checklist抽出粒度のチューニング
- Learning Backlog
- Monthly Learning Review
- Sprint Retrospective
- Portfolio Review
- Career Review

検討中(未着手)

- Learning Contexts データベースをLearner Profileへ統合し、データベース数をさらに減らす案(Sprintの履歴管理をどう扱うか要検討のため保留)

---

# Current Status

Architecture

Completed

Prompt

Completed(SaturdayもSundayも埋め込み方式で統一)

Specification

Completed

Workflow

Migrated to Claude Scheduled Tasks

Learning Progress Sync

Implemented

Learning Checklist

Implemented

---

# Current Version (as of v2.1.0)

v2.1.0

Status

Stable

(superseded by v2.2.0 — see top of this document for the current version)

---

# v2.0.0

## Release Date

2026-08

---

## Overview

実行基盤を

n8n

から

Claude Scheduled Tasks (Claude Code Remote trigger)

へ完全移行しました。

外部AI API(OpenAI)への依存をなくし、

Claude自身がレポートを執筆する構成へ変更しました。

あわせて、学習計画の「実行できたかどうか」を追跡する

Learning Checklist

を新規追加し、学習進捗をLearner Profileへ還元する

Learning Progress Sync

を構築しました。

---

# Added

## Learning Checklist Database

新規追加

Sunday Learning Planner実行時に、

その週のSunday Learning Reportから

3〜6件の具体的な実践項目を抽出し、

チェックリストとしてNotionに保存する。

保持する情報

- Name
- Done
- Reflected
- Source Report Date

チェックは学習者本人が手動で行う。

---

## Learning Progress Sync (Saturday Step 0)

新規追加

目的

Learning Checklistのうち

Done = true かつ Reflected = false

の項目を(特定の週に限らず過去分すべて)取得し、

Learner Profile

Current Sprint (Learning Contexts)

へ内容を反映する。

反映済み項目は Reflected = true に更新する。

---

## Learning Completed Flag

新規追加

ある週のLearning Checklist項目が全件Doneになった時点で、

対応するSunday Learning Reportページの

Learning Completed

をtrueにする。

Sunday Learning Reportsデータベースへ新規プロパティとして追加。

---

# Changed

## Execution Platform

n8n Workflow

から

Claude Scheduled Task

へ変更。

Workflowの可視化(n8nキャンバス)は失われるが、

Prompt自体がWorkflowの定義を兼ねる構成になった。

---

## AI Engine

OpenAI API

から

Claude自身

へ変更。

外部AI APIキーの管理が不要になった。

---

## Article Collection (Saturday)

RSSフィードへの直接アクセス(WebFetch)

から

ドメイン制限付きWebSearch

へ変更。

理由

無人実行中にWebFetchの権限確認プロンプトが発生し、

処理が数分〜数十分単位で遅延する事例が発生したため。

WebSearchは同種の権限プロンプトを発生させず、

処理時間が安定した。

---

## Report Format (Saturday)

「なぜ重要？」と「実務ではどう使われる？」のセクションを統合。

Mermaid図解セクションを完全廃止。

Priority・Tagsを固定値からClaudeによる動的判断へ変更。

ただし、Tagsのスキーマ変更(選択肢追加)は

無人実行では絶対に行わない設計とした(下記 Unattended Safety 参照)。

---

## Prompt Management (Saturday)

Notion AI Prompts Databaseからの動的取得をやめ、

Prompt本文をトリガー定義に直接埋め込む方式へ変更。

理由

無人実行における可用性を優先し、

Notion側の状態に依存する箇所を減らすため。

Sunday側は引き続きAI Prompts Databaseから取得する。

---

# Improved

## Unattended Safety

無人・自動実行と相性が悪い操作を明文化し、禁止した。

禁止事項

- Notionデータベースのスキーマ変更(列・選択肢の追加)を通常運用フローで行わない
- RSS/Atomフィードへの直接WebFetchを行わない(WebSearchで代替)

これらは「実行に失敗する」のではなく

「承認待ちのまま無期限にハングする」形で失敗するため、

無人実行では特に注意が必要と判断した。

---

## Duplicate Prevention

Saturday Workflowの重複防止チェックはそのまま維持。

Sunday Workflowには重複防止チェックが存在しない点は

既知の制約として記録する(Future Roadmap参照)。

---

# Directory Structure

変更なし。

`workflows/*.json` はn8n時代の成果物として保持するが、

v2.0.0以降のPromptの正とはしない。

正は `docs/prompts/implementations/*-v2.0.md`。

---

# Breaking Changes

n8n Workflowは無効化した(削除はしていない)。

Notionデータベースのスキーマ(プロパティ)は後方互換を維持。

Sunday Learning ReportsデータベースにLearning Completedプロパティを追加(既存データへの影響なし)。

---

# Future Roadmap

予定

- Sunday Workflowへの重複防止チェック追加
- Learning Checklist抽出粒度のチューニング
- Learning Backlog
- Monthly Learning Review
- Sprint Retrospective
- Portfolio Review
- Career Review

---

# Current Status

Architecture

Completed

Prompt

Completed

Specification

Completed

Workflow

Migrated to Claude Scheduled Tasks

Learning Progress Sync

Implemented

Learning Checklist

Implemented

---

# Version (as of v2.0.0 release)

v2.0.0

Status

Stable

(superseded by v2.1.0 — see top of this document for the current version)

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

# Version (as of v1.1 release)

v1.1.0

Status

Stable

(superseded by v2.0.0 — see top of this document for the current version)
