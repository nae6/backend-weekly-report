# Sunday Learning Planner Specification v2.1

## Overview

v2.1.0で、System Prompt / User Prompt Templateの取得方式を変更した。このドキュメントはv2.0仕様との差分に焦点を当てる。共通する目的・出力内容・Learning Checklist生成の仕様はv2.0仕様を参照。

---

# What Changed from v2.0

## Prompt Source

Notion AI Prompts Databaseから動的取得（Name="Sunday Learning Planner" AND State="Active"を検索）

↓

トリガー定義に直接埋め込み

v2.0.0でSaturdayに適用したのと同じ変更を、v2.1.0でSundayにも適用した。

理由

AI Prompts Databaseには実質Sunday用の1行しか存在しておらず、「1行しかない可変データのために毎回Notionへ問い合わせる」コストに見合う価値がなかったため。埋め込み方式へ統一することで:

- 実行のたびのNotion問い合わせが2回減る（`notion-query-data-sources`での検索 + `notion-fetch`での本文取得）
- Report Type / Version / Activeの設定ミスによる不動作の余地がなくなる
- SaturdayとSundayでプロンプトの持ち方が揃う

Prompt本文自体（System Prompt / User Prompt Templateの内容）は変更していない。プレースホルダー置換（`{{currentSprint}}`等）というテンプレート機構は廃止し、埋め込んだSystem Promptの直後に「以下の情報を入力として扱ってください」という形で直接説明する方式に変えた（Saturdayの「日付」置換の説明方式と統一）。

---

## AI Prompts Database の扱い

v2.1.0以降、このデータベースはSaturday・Sundayどちらのトリガーからも参照されない。

Notion上のデータベース自体は自動では削除されない。残すか削除するかは運用者の判断に委ねる。

---

# Unaffected

以下はv2.0から変更していない。

- Learning Checklist生成（Step 4）の仕様
- Sunday Learning Reportsへの保存形式（" Version" / " Source Report Date"の命名を含む）
- 重複防止チェックが存在しないという既知の制約

---

# Version

Current Version

v2.1.0

Status

Implementation Complete

Supersedes

v2.0
