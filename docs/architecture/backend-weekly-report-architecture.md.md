# Backend Weekly Industry Report Architecture

## Overview

Backend Weekly Industry Report は、毎週土曜日にバックエンド業界の最新情報を収集・分析し、今週の技術動向を整理するワークフローです。

このワークフローは、ニュースを一覧化することではなく、

- 今週何が起きたのか
- なぜ重要なのか
- 実務へどのような影響があるのか

を整理した Industry Report を生成することを目的としています。

生成されたレポートは Sunday Learning Planner の入力として利用されます。

---

# Architecture

```text
        Step 0: Learning Progress Sync
     (reflect completed checklist items
      into Learner Profile / Current Sprint)
                          │
                          ▼
        Step 0.5: GitHub Activity Sync
     (reflect last 7 days of commits
      into Learner Profile / Current Sprint)
                          │
                          ▼
        Duplicate Report Check (same day)
                          │
                          ▼
     WebSearch 5 Official Sources (parallel,
        domain-restricted, no raw WebFetch)
                          │
                          ▼
                Remove Duplicates
                          │
                          ▼
        Claude writes the Industry Report
           (System Prompt embedded in
              the trigger, not fetched)
                          │
                          ▼
      Backend Weekly Industry Report
                          │
                          ▼
   Assign Priority + Tags + Role Model
            + Deep Dive Topic
     (existing Tags options only — no
        schema changes at runtime)
                          │
                          ▼
                     Notion
```

---

# Responsibilities

本ワークフローの責務

- 学習進捗の反映（Step 0. Learning Progress Sync）
- GitHub活動の反映（Step 0.5. GitHub Activity Sync）
- 記事収集（WebSearch）
- Backend関連記事抽出
- 重複除去
- 技術分析
- 技術トレンド整理
- 実務への影響整理
- Role Model / Deep Dive Topic の選定
- Industry Report生成
- Notion保存

対象外

- Current Sprint分析
- 学習計画
- 学習テーマ決定
- TODO生成

---

# Inputs

## Search Results (WebSearch)

情報源ごとにドメイン制限付きWebSearchで取得する。

含まれる情報

- Title
- URL
- Snippet（概要）

RSS/AtomフィードそのものへのWebFetchアクセスは行わない（Unattended Safety、下記参照）。

---

## System Prompt

トリガー定義に直接埋め込まれている。

Notion AI Prompts Databaseからの動的取得は行わない（v2.1.0からはSunday側も同じ方式）。

---

## Learning Checklist (Step 0 の入力)

Notion Learning Checklistデータベースから、

Done = true かつ Reflected = false

の項目を全期間分取得する。

---

## GitHub Commits (Step 0.5 の入力)

`nae6/*` の各リポジトリ（`backend-weekly-report` 自体を除く）から、直近7日以内のコミットを取得する。

実行環境で許可されていないリポジトリは取得できないため、スキップする。

---

# Processing

## 0. Learning Progress Sync (Step 0)

Learning Checklistの未反映完了項目を取得し、

Learner Profile

Current Sprint (Learning Contexts)

へ反映する。

反映済み項目は Reflected = true に更新する。

ある週の全項目がDoneになっていれば、

対応するSunday Learning Reportの Learning Completed を true にする。

該当項目が0件の場合は何もせず、後続の処理へ進む。

Learner Profileの "Current Skills" は「◆カテゴリ名: 内容」のブロック単位で追記し、"Current Focus" は短いポインタとして維持する。

Current SprintはDefinition of Doneのチェック更新と「## 進捗反映ログ」への日付付き追記で反映する。

---

## 0.5. GitHub Activity Sync (Step 0.5)

直近7日以内にコミットがあったリポジトリについて、何を実装・学習したかをリポジトリ単位・テーマ単位で要約し、

Step 0と同じルールでLearner Profile / Current Sprintへ反映する。

Step 0で反映済みの内容とは重複させず統合し、矛盾する場合は直近のコミット内容を優先する。

Step 0の結果に関わらず毎回実行する。失敗しても致命的エラーとせず、後続の処理へ進む。

---

## 1. Duplicate Check

本日の日付のBackend Weekly Reportが既に存在するか確認する。

存在すれば新規作成せず、既存ページを報告して終了する。

---

## 2. Collect Articles (WebSearch)

5つの情報源についてドメイン制限付きWebSearchを並列実行する。

---

## 3. Remove Duplicate Topics

同一テーマの記事を統合する。

---

## 4. AI Analysis

Claude自身が記事を分析し

- 技術的重要性
- 実務への影響
- 初学者向け解説

を生成する。外部AI APIは使用しない。

---

## 5. Report Generation

Backend Weekly Industry Report を生成する。

Role Model / Deep Dive Topic は、紹介する価値のある人物・技術が見つからない週は無理に選ばない（本文に「今週は該当なし」とだけ書く）。

---

## 6. Priority / Tags Assignment

Priorityは内容に応じて動的判断する。

Tagsは既存の選択肢の中からのみ選択する。

新しい技術名に対応する選択肢がなくても、スキーマ変更は行わない（Unattended Safety）。

あわせて、Role Model / Deep Dive Topic プロパティに④・⑤で取り上げた人物名・技術名を書き込む（該当なしの週は空欄）。

---

## 7. Save

Notionへ保存する。

---

# Outputs

生成される成果物

Backend Weekly Industry Report

内容

- ① 今週最重要ニュース（基準3件、最大6件）
- ② 技術トレンド
- ③ 関連技術
- ④ Role Model（今週参考にできる人物）
- ⑤ Deep Dive Topic（今週の深掘り技術）
- ⑥ 今週の総括

Notionプロパティ

- Priority / Tags（動的判断、既存Tags選択肢のみ）
- Role Model / Deep Dive Topic（該当なしの週は空欄）

Mermaid図解セクションはv2.0.0で完全廃止した。

---

# Consumers

生成された Industry Report は

Sunday Learning Planner

のみが利用する。

```text
Backend Weekly Industry Report

        │

        ▼

Sunday Learning Planner

        │

        ▼

Sunday Learning Report
```

---

# AI Prompt Strategy

v1.0まではNotion AI Prompts Databaseから取得していたが、

v2.0.0でSystem Promptをトリガー定義に直接埋め込む方式へ変更した。

理由

無人実行における可用性を優先し、

Notion側の状態（Active/Versionフラグの設定ミスなど）に依存する箇所を減らすため。

Prompt変更時は、トリガー定義そのものを更新する。

---

# Unattended Safety

このワークフローは人が張り付かない前提で実行される。

以下は通常運用フローで行わない。

- Tags列のスキーマ変更（notion-update-data-source による選択肢追加）
- RSS/AtomフィードへのWebFetch直接アクセス

いずれも、失敗時に処理が止まるのではなく

承認待ちのまま無期限にハングするため、無人実行と相性が悪い。

前者は「既存選択肢のみ使用・空欄許容」、

後者は「WebSearchで代替」することで回避している。

---

# Design Principles

## Separation of Concerns

Industry Report

↓

Learning Report

を完全に分離する。

---

## Single Responsibility

Saturday Workflow は

業界分析だけを担当する。

---

## Source Priority

一次情報を最優先する。

優先順位

1. Official Docs
2. Release Notes
3. GitHub Release
4. Vendor Blog
5. Tech Blog

---

## Future Expansion

将来追加予定

- Monthly Industry Report
- Quarterly Technology Review
- Annual Trend Report

現在のWorkflowへ影響を与えない構造とする。

---

# Version

Current Version

v2.3.0

Status

Implementation Complete（Step 0.5 GitHub Activity Sync、Role Model / Deep Dive Topic を反映）

詳細は [spec v2.3](../prompts/specification/backend-weekly-report-spec-v2.3.md) を参照。
