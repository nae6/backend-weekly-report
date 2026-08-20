# Sunday Learning Planner — Trigger Definition v2.0

## About this document

v1.1までは、n8n Workflowが処理フローを担い、このディレクトリのファイルには「OpenAIへ送るSystem Prompt本文」だけが記録されていた。

v2.0.0では処理フロー自体がトリガー定義（下記）に統合されており、これ単体がWorkflowと連携ロジックの両方を兼ねる。実行基盤はClaude Code Remoteのscheduled task（`trig_01FxupVWtsLiZBeEQ6DXSLtp`、毎週日曜20:00 JST）。

System Prompt / User Prompt Template自体はv1.1から変更なく、引き続きNotion AI Prompts Databaseから動的取得する（Saturdayとは異なり、埋め込み方式へは移行していない）。

以下は実際にトリガーへ設定されているプロンプト全文（2026-08-19時点、Learning Checklist生成ステップを反映した最新版）。

---

## Trigger Prompt (full text)

```text
あなたは以下の自動化タスクを完全に自律的に実行してください(n8nワークフロー「Sunday Learning Planner」からの移行後継版です)。実行日時は日本時間(Asia/Tokyo)の毎週日曜20:00です。NotionのMCPツールを使用してください。

## 手順

### 1. 必要な情報をNotionから収集する

1. データソース collection://3bca2d2d-0b55-8053-8402-000b39141a53 (「Backend Weekly Reports」)から、Report Type = "Industry" で Date が最も新しいページを1件取得し、そのページ本文(Markdown)を取得する → weeklyReport とその日付(sourceReportDate)
2. データソース collection://3bda2d2d-0b55-80bf-beb3-000bae9d8b76 (「Learning Contexts」)から、Status = "Active" のページを1件取得(なければ空)。あればそのページ本文(Markdown)を取得 → currentSprint。なければ currentSprint は空文字列。
3. データソース collection://5c72158b-c6b3-411b-9a41-bfa0f57c76bf (「Learner Profile」)から、Name = "Learner Profile" の行を1件取得し、そのプロパティ "Learner Profile" → learnerProfile、"Current Skills" → currentSkills、"Current Focus" → currentFocus を取得。
4. データソース collection://3bda2d2d-0b55-80ea-bbeb-000b2481e50d (「AI Prompts」)から、Name = "Sunday Learning Planner" かつ State = "Active" の行を1件取得し、プロパティ "System Prompt" → systemPrompt、"User Prompt Template" → userPromptTemplate を取得。

### 2. プロンプトを組み立てて自分自身で実行する
userPromptTemplate 内の以下のプレースホルダーを実際の値で置換してください。
- {{currentSprint}} → currentSprint
- {{currentFocus}} → currentFocus
- {{learnerProfile}} → learnerProfile
- {{currentSkills}} → currentSkills
- {{weeklyReport}} → weeklyReport
- {{reportDate}} → 日本時間での本日の日付(YYYY-MM-DD)

置換後の文章を「ユーザーからのリクエスト」、systemPromptを「あなたの振る舞いのルール」として扱い、あなた自身が(外部AI APIを使わず)Sunday Learning Reportを生成してください。systemPrompt内のOutput Format指示に厳密に従い、Markdown形式で出力してください。

### 3. Notionに保存する
データソース collection://3bda2d2d-0b55-8056-aea7-000b1a322137 (「Sunday Learning Reports」)に新規ページを作成してください。

- Title: "Sunday Learning Report {今日の日付 YYYY-MM-DD}"
- Date: 今日の日付
- Status: "Published"
- " Version" (先頭に半角スペースあり): "1.0.0"
- " Source Report Date" (先頭に半角スペースあり): 手順1で取得した元のBackend Weekly ReportのDate
- content: 生成したSunday Learning ReportのMarkdown全文

### 4. 学習チェックリストを作成する
手順3で生成したSunday Learning Reportの中から、今週の具体的な実践項目・タスク(例: 特定の技術を試す、特定の作業を行う、特定のドキュメントを読むなど、後で「終わった/終わっていない」を自分でチェックできる粒度のもの)を3〜6個程度抽出してください。

まず、Notionツールで以下のURLをfetchし、そのデータソースID(collection://形式)を特定してください:
https://app.notion.com/p/3c1a2d2d0b5580bcb2f1f8d800ba8ed1?v=3c1a2d2d0b5580a1b6a8000ce304284a
(このデータベースは「Learning Checklist」という名前です)

特定できたら、抽出した項目それぞれについて、そのデータソースに新規ページを1件ずつ作成してください:
- Name (title): 項目の内容を簡潔に(1行、40文字程度を目安)
- Done (checkbox): false(未チェック)
- Reflected (checkbox): false(未チェック)
- Source Report Date (date): 今日の日付(手順3のSunday Learning Reportと同じ日付)

このステップで問題が起きても(データベースが見つからない等)、致命的なエラーとして扱わず、その旨を完了報告に含めた上で処理を継続してください。

### 5. 完了報告
最後に、作成したNotionページのURL(Sunday Learning Report本体、および学習チェックリストに追加した項目数)を含めて簡潔に完了を報告してください(このタスクはバックグラウンドで自動実行されるため、詳細な経過報告は不要です)。もし元となるBackend Weekly Reportが見つからなかった場合は、無理に生成せず、その旨だけ報告して終了してください。
```

---

## Notion connector permissions

`notion-create-pages` と `notion-update-page` を `always_allow` に設定している（Sunday Learning ReportおよびLearning Checklist項目の作成が無人実行で承認プロンプトに阻まれないようにするため）。

---

## Known Limitation

このトリガーには重複防止チェックがない。同日に複数回手動実行すると、Sunday Learning ReportとLearning Checklist項目が重複して作成される（[spec v2.0](../specification/sunday-learning-planner-spec-v2.0.md) の Known Limitation を参照）。

---

## Version

Current Version

v2.0.0

Supersedes

v1.1
