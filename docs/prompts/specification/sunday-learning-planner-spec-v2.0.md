# Sunday Learning Planner Specification v2.0

## Overview

v2.0.0で、実行基盤をn8n WorkflowからClaude Scheduled Task（Claude Code Remote trigger）へ移行した。

このドキュメントはv1.1仕様との差分に焦点を当てる。共通する目的・出力内容はv1.1仕様を参照。

---

# What Changed from v1.1

## Execution

n8n Workflow

↓

Claude Scheduled Task（毎週日曜20:00 JST、無人実行）

---

## AI Engine

OpenAI API

↓

Claude自身（外部AI APIを使用しない）

Prompt取得元（Notion AI Prompts Database）はv1.1から変更なし。System Prompt / User Prompt Templateを組み立てたうえで、Claude自身がそれに従って実行する。

---

## Learning Checklist Generation (新規, Step 4)

Sunday Learning Reportを生成・保存した直後に、以下を行う。

1. レポートの中から、具体的で「終わった/終わっていない」を判定できる粒度の実践項目を3〜6件抽出する
2. Notion Learning Checklistデータベースに、抽出した項目ごとに新規ページを作成する

  - Name（項目内容、1行40文字程度が目安）
  - Done（初期値 false）
  - Reflected（初期値 false）
  - Source Report Date（そのSunday Learning Reportと同じ日付）

このステップで問題が起きても致命的エラーとせず、完了報告にその旨を含めたうえで処理を継続する。

---

## Learning Checklistの役割

Sunday Learning Reportは「計画」であり、実行されたかどうかまでは分からない。

Learning Checklistは、その計画を実行可能な単位に分解し、学習者が週の間に手動でチェックできる形にする。

チェックされた項目は、次のSaturday Step 0（Learning Progress Sync、[backend-weekly-report-spec-v2.0.md](backend-weekly-report-spec-v2.0.md) 参照）でLearner Profile / Current Sprintへ反映される。

---

# Known Limitation

Sunday Workflowには、Saturday Workflowと異なり**重複防止チェックが存在しない**。

同日に複数回実行すると、Sunday Learning Reportおよび Learning Checklist項目が重複して作成される。

v2.0.0では未対応（Future Roadmap参照）。

---

# Version

Current Version

v2.0.0

Status

Implementation Complete

Supersedes

v1.1
