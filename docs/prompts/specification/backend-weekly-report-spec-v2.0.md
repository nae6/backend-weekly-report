# Backend Weekly Industry Report Specification v2.0

## Overview

v2.0.0で、実行基盤をn8n WorkflowからClaude Scheduled Task（Claude Code Remote trigger）へ移行した。

このドキュメントはv1.0仕様との差分に焦点を当てる。共通する目的・出力内容はv1.0仕様を参照。

---

# What Changed from v1.0

## Execution

n8n Workflow

↓

Claude Scheduled Task（毎週土曜20:00 JST、無人実行）

トリガー定義そのものがWorkflow定義を兼ねる。n8nキャンバスに相当するものは存在しない。

---

## AI Engine

OpenAI API

↓

Claude自身（外部AI APIを使用しない）

トリガーを実行しているClaudeが、記事分析からレポート執筆までを直接行う。

---

## Article Collection

RSSフィードへの直接アクセス（WebFetch/XMLパース）

↓

ドメイン制限付きWebSearch

理由: 無人実行中にWebFetchの権限確認プロンプトが発生し、処理が遅延する事例が発生したため。

対象は変わらず5情報源（PHP Official / Laravel News / Symfony Blog / AWS What's New / Docker Blog）。

---

## Prompt Source

Notion AI Prompts Databaseから動的取得

↓

トリガー定義に直接埋め込み

理由: 無人実行における可用性を優先し、Notion側の状態（Active/Versionフラグの設定ミス等）に依存する箇所を減らすため。

Prompt本文を変更する場合は、トリガー定義そのものを更新する。

---

## Diagram Rules

v1.0では「必要な場合のみMermaidを出力する」というルールだったが、v2.0.0では完全に廃止した。

理由: 生成時間の短縮、および図解が実際にはほとんど使われていなかったため。

---

## Priority / Tags

v1.0では固定値だったPriorityとTagsを、v2.0.0でレポート内容に応じた動的判断に変更した。

ただし、Tagsは既存の選択肢（Notion側で事前定義済み）の中からのみ選択する。

新しい技術名に対応する選択肢が存在しない場合でも、**スキーマ変更（notion-update-data-source）は行わない**。

理由: スキーマ変更は承認待ちの権限プロンプトを発生させ、無人実行では無期限にハングするため（下記 Unattended Safety 参照）。

該当する既存タグが1つもない場合は、Tagsを空のままにする。

---

## Learning Progress Sync (Step 0, 新規)

Backend Weekly Industry Reportの生成に先立ち、以下を毎回実行する。

1. Notion Learning Checklistデータベースから、Done = true かつ Reflected = false の項目を全期間分取得する
2. 該当項目が0件なら何もせず、通常のレポート生成処理へ進む
3. 該当項目があれば、その内容をもとにLearner Profile（Current Skills / Current Focus）とCurrent Sprint（Learning Contexts）を更新する
4. 反映した項目のReflectedをtrueに更新する
5. ある週の項目が全件Doneになっていれば、対応するSunday Learning Reportの Learning Completed をtrueにする

このステップは「今週の学習が実際にどこまで進んだか」を、次の業界レポート生成より先に学習者プロファイルへ反映する目的で追加した。

過去のどの週の未反映項目も対象とする（最新週限定ではない）。

---

# Unattended Safety (新規)

このワークフローは人が張り付かない前提で実行される。以下は通常運用フローで**絶対に行わない**。

- Notionデータベースのスキーマ変更（列・選択肢の追加）
- RSS/AtomフィードへのWebFetch直接アクセス

これらの操作は、失敗時に「エラーで終了する」のではなく「承認待ちのまま無期限にハングする」形で失敗する。

通常のtry/catchやリトライロジックでは救えないため、設計段階で発生させないことを原則とする。

回避策

- タグ選択肢がない場合は空欄で処理を続行する（スキーマ変更をしない）
- RSS取得はWebSearchで代替する（WebFetchを使わない）

---

# Duplicate Prevention (v1.0から継続)

本日の日付のBackend Weekly Reportが既に存在する場合、新規作成せず既存ページのURLを報告して終了する。

このチェックはStep 0（Learning Progress Sync）より後、記事収集より前に実行する。

---

# Version

Current Version

v2.0.0

Status

Implementation Complete

Supersedes

v1.0
