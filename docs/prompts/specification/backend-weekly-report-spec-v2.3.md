# Backend Weekly Industry Report Specification v2.3

## Overview

v2.0.0以降、トリガー定義へ直接加えられていた変更を仕様として記録する。

このドキュメントはv2.0仕様との差分に焦点を当てる。実行基盤・Unattended Safety・重複防止など、変更のない項目は[v2.0仕様](backend-weekly-report-spec-v2.0.md)を参照。

実装（トリガープロンプト全文）は [backend-weekly-report-v2.3.md](../implementations/backend-weekly-report-v2.3.md)。

---

# What Changed from v2.0

## Processing Order

```text
Step 0: Learning Progress Sync（Learning Checklist）
↓
Step 0.5: GitHub Activity Sync（新規）
↓
Duplicate Report Check
↓
WebSearch 5 Official Sources
↓
Report Generation
↓
Save to Notion（Priority / Tags / Role Model / Deep Dive Topic）
```

---

## Step 0: Learning Progress Sync（ルールの具体化）

反映先ごとに、書き込み方のルールを追加した。

Learner Profile

- "Current Skills" は「◆カテゴリ名: 内容」形式の複数ブロックで構成する。新しいスキルは既存の該当カテゴリへ追記し、当てはまるカテゴリがない場合のみ新しいブロックを追加する（段落を丸ごと書き直さない）
- "Current Focus" は「Learning Contexts内のどの項目に取り組み中か」を示す1〜2文の短いポインタとして維持する（詳細はLearning Contexts側に書く）

Current Sprint（Learning Contexts）

- 該当するDefinition of Doneのチェックボックスを `[ ]` → `[x]` に更新する
- 「## 進捗反映ログ」の末尾に `### YYYY-MM-DD` 見出しで反映内容を追記する（過去のログは消さない。同じ日付の見出しがあればそこへ追記）
- 進捗の反映のみ行い、Sprint自体の完了判断はしない

---

## Step 0.5: GitHub Activity Sync（新規）

目的

手動のチェック（Learning Checklist）だけでなく、GitHub上で実際に書いたコードからも学習進捗をLearner Profile / Current Sprintへ反映する。

処理

1. `nae6/*` の各リポジトリ（`backend-weekly-report` 自体を除く）から、直近7日以内（日本時間基準）のコミットを取得する
2. コミットがあったリポジトリについて、コミットメッセージと変更ファイルの概要から、今週何を実装・学習したかをリポジトリ単位・テーマ単位で要約する
3. Step 0と同じルールでLearner ProfileとCurrent Sprintへ反映する。Step 0で既に反映済みの内容とは重複させず統合する
4. Step 0とStep 0.5の情報が矛盾する場合は、より具体的で新しい情報（直近のコミット内容）を優先する

制約

- 実行環境で許可されていないリポジトリはAPIリクエスト自体が拒否される。これは実行環境側の制約でありプロンプトでは回避できないため、そのリポジトリは黙ってスキップする
- Step 0の結果に関わらず毎回実行する。問題が起きても致命的エラーとせず、最終報告に記録してStep 1以降へ進む

Sundayへは追加しない（v2.2.0で検討の上見送り。[version-history](../../architecture/version-history.md) を参照）。

---

## Report Output

見出し構成

```text
① 今週最重要ニュース（基準3件、最大6件）
② 今週の技術トレンド
③ 関連技術
④ Role Model（新規）
⑤ Deep Dive Topic（新規）
⑥ 今週の総括（旧④）
```

### ① 今週最重要ニュースの件数

「最大3件」から「基準3件、実務上重要なテーマが3件を超える週は最大6件」へ変更した。

件数を埋めるために重要度の低い記事を追加しない（3件で十分な週は3件のままでよい）。

### ④ Role Model（新規）

構成

- 今週の人物（名前・所属や立場）
- 選定理由（今週の技術トレンドやニュースとの関連）
- 学べること（考え方・仕事の進め方から、今の学習段階で参考にできる点）

選定基準は知名度ではなく「今の学習段階から参考にできるか」。

### ⑤ Deep Dive Topic（新規）

構成

- 今週の深掘り技術
- 選定理由（今週のニュースとの関連）
- 深掘り内容（一次情報ベースの技術解説）
- 実務での使いどころ

Current Sprintとの関連判断はしない（学習計画はSundayの責務のため）。

### 該当なしの扱い

④・⑤とも、紹介する価値のある人物・技術が見つからない週は無理に選ばず、本文に「今週は該当なし」と一行だけ書く。

---

## Notion Properties

Backend Weekly Reportsデータベースの以下のテキストプロパティに書き込むようにした（プロパティ自体はデータベース作成時から存在していたが、v2.0まではどの処理からも書き込まれていなかった）。

| Property | 値 |
| --- | --- |
| Role Model | ④で取り上げた人物名 |
| Deep Dive Topic | ⑤で取り上げた技術名 |

該当なしの週は空欄のままにする（「該当なし」という文字列は入れない）。

スキーマ変更は行っていないため、Unattended Safetyの原則（無人実行中にスキーマを変更しない）には抵触しない。

---

## Final Report

完了報告に、GitHub活動からLearner Profile / Learning Contextsへ反映した内容の概要を含める。

---

# Version

Current Version

v2.3.0

Status

Implementation Complete

Supersedes

v2.0
