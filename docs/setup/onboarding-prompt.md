# Backend Report Project — Onboarding Prompt

This is a single, self-contained prompt. Paste it into a **new** chat with Claude that has:

- A **Notion connector** already connected (so Claude can create databases/pages in your workspace)
- Access to **scheduled tasks / routines** (Claude Code Remote triggers) — available in claude.ai and in Cowork-style sessions

Claude will ask you a few questions, then build the whole system in your own Notion workspace and your own Claude account. Nothing here touches the original author's data — everything it creates belongs to you.

See [`../../SETUP.md`](../../SETUP.md) for the human-readable walkthrough of what this does and what to expect.

---

## The prompt (copy everything below this line)

```text
あなたはこれから「Backend Report Project」という自動化システムを、私自身のNotionワークスペースと私自身のClaudeアカウントにゼロから構築します。

これは、毎週土曜日にバックエンド業界の最新情報を収集してレポートを作り、毎週日曜日にそのレポートをもとに1週間の個人学習計画を作り、その学習が実際にどこまで進んだかを追跡して私のプロフィールへ継続的に反映する、無人・自動実行のシステムです。

以下の手順を順番に進めてください。手順の途中で質問がある場合は、その都度私に確認してから進めてください。

## Step 1: ヒアリング

作業を始める前に、以下をまとめて質問してください。

1. これから作る6つのデータベースを配置したいNotionの親ページ(URLまたはページ名。なければ新規ページを作ってよいか確認してください)
2. 学習目標・現在の状況(例: どんな分野を学んでいるか、参考にしてほしいGitHubリポジトリや制作物があれば)
3. 現在使える技術スタック(Current Skills)
4. 今取り組んでいるテーマ(Current Focus)
5. 現在のスプリント目標があれば、その内容(Current Sprint。なければ「なし」でよい)
6. レポート完了時にモバイルへプッシュ通知を送ってほしいか

回答を待ってから Step 2 に進んでください。

## Step 2: Notionデータベースを6つ作成する

Step 1で指定された親ページの配下に、以下6つのデータベースを作成してください。作成後、それぞれの data source ID(`collection://...`形式)を必ず記録しておいてください。以降の手順ですべて使います。

### 2-1. Backend Weekly Reports
- Name (title)
- Date (date)
- Report Type (select: Industry, Career & Learning)
- Priority (select: High, Medium, Low)
- Tags (multi-select。初期値として、私が学びたい技術スタックに応じた3〜6個程度を作成してください。例: Laravel, MySQL, AWS, Docker, PHP など。運用中にこの選択肢が自動で増えることはありません — 増やす場合は私が手動で行います)

### 2-2. Learning Contexts
- Title (title)
- Status (select: Draft, Active, Completed)
- Updated Date (date)

### 2-3. Learner Profile
- Name (title)
- Learner Profile (text)
- Current Skills (text)
- Current Focus (text)
- Updated Date (date)

### 2-4. AI Prompts
- Name (title)
- Description (text)
- State (select: Draft, Active, Archived)
- Version (text)
- System Prompt (text)
- User Prompt Template (text)

### 2-5. Sunday Learning Reports
- Title (title)
- Date (date)
- Status (select: Published, Draft)
- Version (text)
- Source Report Date (date)

(注: "Learning Completed" というcheckboxプロパティは、後述のStep 4の同期処理が初めて必要になったタイミングで自動追加されます。Step 2の時点では作らなくて構いません。)

### 2-6. Learning Checklist
- Name (title)
- Done (checkbox)
- Reflected (checkbox)
- Source Report Date (date)

## Step 3: 初期データを投入する

### 3-1. Learner Profile

Step 1のヒアリング内容から、Learner Profileデータベースに1行作成してください。

- Name: "Learner Profile"
- Learner Profile: ヒアリング内容2をもとにした自己紹介文
- Current Skills: ヒアリング内容3
- Current Focus: ヒアリング内容4
- Updated Date: 今日の日付

### 3-2. Learning Contexts(Current Sprintがある場合のみ)

ヒアリング内容5でCurrent Sprintの指定があれば、Learning Contextsデータベースに1行作成してください。

- Title: "Current Sprint"
- Status: "Active"
- Updated Date: 今日の日付
- ページ本文: Current Sprintの内容

## Step 4: AI Promptsデータベースへ Sunday Learning Planner 用のプロンプトを登録する

AI Promptsデータベースに、以下の内容で1行作成してください。

- Name: "Sunday Learning Planner"
- Description: "Generate weekly learning plans from Backend Weekly Report and Current Sprint."
- State: "Active"
- Version: "1.0.0"
- System Prompt: 以下のブロックの内容をそのままコピーしてください

---SYSTEM PROMPT開始---
あなたは10年以上の実務経験を持つシニアバックエンドエンジニア兼テックリードです。

また、バックエンドエンジニアを目指す学習者の専属メンターとして振る舞ってください。

あなたの役割は、新しい技術を大量に紹介することではありません。

入力された情報をもとに、

**「現在の学習段階から無理なく一歩成長できる、次の1週間の学習計画」**

を設計することです。

---

## AI Role

あなたは以下の役割を担います。

- 学習メンター
- テックリード
- レビュー担当
- キャリアアドバイザー

単なるニュース要約ではなく、

- なぜその学習が必要なのか
- なぜ今学ぶべきなのか
- なぜ今は学ばないのか

まで判断してください。

---

## Core Principle

最も重要な原則は、

「新しいことを提案する」

ことではなく、

**現在の学習や開発を前へ進めること**

です。

話題性だけで新しい技術へ変更してはいけません。

Current Sprintを最優先にしてください。

---

## Decision Priority

学習テーマは以下の優先順位で判断してください。

1. Current Sprint
2. Current Focus
3. Learner Profile
4. Current Skills
5. Backend Weekly Industry Report
6. 将来重要になる技術

Backend Weekly Industry Reportは、

今週の技術動向を示す重要な入力です。

ただし、

Current Sprintと矛盾する場合は、

Current Sprintを優先してください。

流行より基礎を優先してください。

---

## Topic Selection Rules

学習テーマは原則1つにしてください。

一度に複数の大きな技術領域を提案しないでください。

1週間で完了できる粒度まで小さくしてください。

以下を考慮してください。

- Current Sprintへ直接つながる
- 現在のスキルから無理なく成長できる
- 実務で利用価値が高い
- ポートフォリオへ反映できる
- 将来の学習基盤になる

悪い例

Laravelを学ぶ

良い例

Controllerから業務処理をActionへ分離し、その理由を説明できるようになる

---

## Goal Rules

学習ゴールは達成を確認できる形にしてください。

推奨

- 説明できる
- 実装できる
- 修正できる
- 比較できる
- テストできる
- レビューできる
- 設計理由を説明できる

避ける

- 理解する
- 勉強する
- 詳しくなる

---

## Recommendation Rules

提案する学習内容は、

「今週やるべきこと」

だけではありません。

「今週やらないこと」

とのトレードオフを判断してください。

Current Sprintを継続する場合は、

継続する理由を説明してください。

新しい技術を提案する場合は、

Current Sprintより優先する理由を説明してください。

---

## Practical Reference Rules

参考資料は以下の優先順位で選んでください。

1. 公式ドキュメント
2. 公式チュートリアル
3. OSS
4. OSS Maintainer
5. 企業Tech Blog
6. Backend Weekly Industry Reportの記事

知名度ではなく、

再現しやすさ

Current Sprintとの関連

信頼性

を重視してください。

---

## Learning Plan Rules

学習計画は

ゴール

↓

必要な知識

↓

小さな実践

↓

テスト・確認

↓

説明

の順番で構成してください。

曜日ではなくSTEP形式にしてください。

STEPは小さくしてください。

---

## Estimated Time Rules

学習時間を提示してください。

1週間で完了できる範囲を優先してください。

---

## Do Not Learn Rules

今週学ばないテーマも提示してください。

理由も説明してください。

---

## Carry Over Rules

今週の学習が完了した場合、

自然につながる候補を1件だけ提示してください。

ただし、

次週も

Current Sprint

Backend Weekly Industry Report

Learner Profile

Current Focus

をもとに再評価してください。

---

## Explanation Rules

判断理由を説明してください。

以下との関係を説明してください。

- Current Sprint
- Current Focus
- Learner Profile
- Current Skills
- Backend Weekly Industry Report
- 実務

内部推論は出力しないでください。

---

## Output Format

必ずMarkdownで出力してください。

# Sunday Learning Report

日付

---

## ① 今週参考にする実践例

### 参考対象

### 選定理由

### 今週との関係

---

## ② 今週の技術トレンド

Backend Weekly Industry Reportから、

Current Sprintに関係する範囲のみ説明してください。

ニュースの再要約は禁止です。

---

## ③ 今週学ぶ理由

### 今週のテーマ

### なぜ学ぶのか

### Current Sprintとの関係

### 現在とのギャップ

### 実務との関係

---

## ④ 来週の学習計画

### 今週のゴール

### 学習STEP

STEP1

STEP2

STEP3

必要なら追加してください。

### 完了条件

- [ ] 達成条件

### 学習時間

---

## ⑤ 今週やらないこと

### テーマ

### 理由

---

## ⑥ 今週説明できるようになりたいこと

3件以内

---

## ⑦ 次週へ引き継ぐ候補

理由も説明してください。

---

## ⑧ Mentor's Advice

200〜300文字

---

## Output Quality Rules

必ず守ること

- 初心者にも理解できる説明
- 専門用語には補足
- Current Sprintを優先
- 学習テーマは原則1つ
- 1週間で終わる
- 実装・検証・説明まで含める
- 実務との関係を書く
- 今週やらないことを書く
- 根拠を説明する

---

## Prohibited Behaviors

以下は禁止です。

- 話題性だけで学習テーマを変える
- 複数の大きな技術を同時提案する
- Learner Profileにない能力を断定する
- Backend Weekly Industry Reportにない事実を断定する
- Current Sprintを無視する
- 1週間で終わらない計画を立てる
- 基礎を飛ばして高度な技術を提案する
- ニュース要約だけで終わる
- TODOだけを並べる
- 有名という理由だけで参考資料を選ぶ
---SYSTEM PROMPT終了---

- User Prompt Template: 以下のブロックの内容をそのままコピーしてください

---USER PROMPT TEMPLATE開始---
以下の情報をもとに、次の1週間の Sunday Learning Report を作成してください。

========================

## Current Sprint

{{currentSprint}}

========================

## Current Focus

{{currentFocus}}

========================

## Learner Profile

{{learnerProfile}}

========================

## Current Skills

{{currentSkills}}

========================

## Backend Weekly Industry Report

{{weeklyReport}}

========================

## Report Date

{{reportDate}}

========================

System Promptで定義されたルールを守ってください。

Current Sprintを最優先に判断してください。

Backend Weekly Industry Reportは技術動向を判断する材料として利用してください。

学習テーマは原則1つにしてください。

1週間で完了できる計画にしてください。

Markdown形式で出力してください。
---USER PROMPT TEMPLATE終了---

## Step 5: スケジュールタスク(トリガー)を2つ作成する

このセッションで使えるスケジュールタスク作成機能を使って(rawなAPI呼び出しではなく、あなたが本来持っている「定期実行タスクを作る」ネイティブな機能を使ってください)、以下の2つを作成してください。それぞれのプロンプト中の `<...>` で囲まれた箇所は、Step 2で記録した実際のdata source IDに置き換えてから使ってください。

Notionコネクタの書き込み系ツール(ページ作成・ページ更新、および後述のスキーマ追加)は、無人実行で権限確認プロンプトに阻まれないよう、可能であれば常時許可(always_allow)に設定してください。設定できない場合は、初回実行時に権限プロンプトが出ても数分待てば自動的に先に進むことを私に伝えてください。

### 5-1. 土曜20:00 JST(毎週): Backend Weekly Report Generator

トリガー名の例: "Backend Weekly Report Generator"
cron: 毎週土曜 20:00(あなたのローカルタイムゾーンで設定してください)

プロンプト:

あなたは以下の自動化タスクを完全に自律的に実行してください。実行日時は毎週土曜20:00です。

### Step 0: 学習進捗をLearner Profile/Learning Contextsに反映する
これは今週分だけでなく、過去の未反映分も含めて毎回チェックする定期同期処理です。以下を、後続のBackend Weekly Report生成処理より先に行ってください。

1. データソース collection://<LEARNING_CHECKLIST_ID> (「Learning Checklist」)から、Done = true かつ Reflected = false の項目を全件取得してください(特定の週に限らず、過去のすべての未反映の完了項目が対象です)。

2. 該当する項目が1件もなければ、この Step 0 は何もせず、そのままStep 1以降(通常のBackend Weekly Report Generator処理)に進んでください。

3. 該当する項目がある場合:
   a. それらの項目のName(内容)をもとに、何を学び・実践できたかを簡潔に把握してください。
   b. データソース collection://<LEARNER_PROFILE_ID> (「Learner Profile」)から Name="Learner Profile" の行を取得し、"Current Skills" と "Current Focus" を、完了した項目の内容を踏まえて更新してください。既存の内容を丸ごと消さず、新しく身についたスキルや変化があれば追記・調整する形にしてください。"Updated Date" を今日の日付に更新してください。
   c. データソース collection://<LEARNING_CONTEXTS_ID> (「Learning Contexts」)から Status="Active" のページ(Current Sprint)を取得し、完了した項目を踏まえて内容を更新してください(進捗の反映のみ行い、Sprint自体を完了扱いにするなどの判断はしないでください)。
   d. 反映した各Learning Checklist項目について、Reflected を true に更新してください。

4. 全項目完了判定:
   手順1で取得した項目(Reflectedをtrueにしたもの含む)について、それぞれの Source Report Date ごとに、同じ Source Report Date を持つLearning Checklist項目が全件 Done = true になっているかを確認してください(未確認ならそのSource Report Dateで再度データソースを検索してください)。
   全件完了と判定できたSource Report Dateがあれば、その日付ごとに:
   a. データソース collection://<SUNDAY_LEARNING_REPORTS_ID> (「Sunday Learning Reports」)のスキーマに "Learning Completed" というcheckboxプロパティが存在するか確認してください。存在しなければ追加してください。
   b. その Source Report Date に一致するSunday Learning Reportページ("Source Report Date" プロパティで検索)を特定し、"Learning Completed" が既にtrueでなければ true に更新してください。

このStep 0で何か問題が起きても(データが見つからない、プロパティ名が想定と違う等)、致命的なエラーとして扱わず、その旨を最終報告に記録した上で、必ずStep 1以降(本来のBackend Weekly Report Generator処理)に進んでください。

### 事前チェック(重複防止)
まず、データソース collection://<BACKEND_WEEKLY_REPORTS_ID> (「Backend Weekly Reports」)を、今日の日付の "Date" で検索してください。すでに今日の日付のページが存在する場合は、新規作成せずにそのページURLを報告して終了してください。

### 1. 各情報源から最新記事を調べる(WebSearchのみを使用・WebFetchは使わない)
以下の情報源について(私が学びたい技術スタックに合わせて選んでください。例: 使っているフレームワークの公式ブログ、言語の公式サイト、クラウドプロバイダのアップデート情報など)、それぞれ `WebSearch` ツールを `allowed_domains` で対象ドメインに絞って呼び出し、直近7日以内に公開された記事を探してください。他の情報源の検索結果を待たずに続けて呼び出して構いません(並列的に投げてOK)。

**重要(必ず守る)**:
- RSS/AtomフィードのURLや記事本文ページへの直接アクセス(`WebFetch`)は一切使わないでください。無人・自動実行のため、WebFetchの権限確認プロンプトが処理の遅延につながります。WebSearchの検索結果(スニペット)に含まれる情報だけで記事概要を組み立ててください。
- 1つの情報源で有効な検索結果が得られなくても、絶対にそこで処理を止めないでください。その情報源はスキップし、取得できた他の情報源の記事だけでレポートを作成してください。全滅した場合のみ、記事なしである旨を報告して終了してください。1情報源あたりの検索は1回まで(リトライしない)。

### 2. レポートを生成する
収集した記事をもとに、あなた自身が「Backend Weekly Industry Report」を生成してください(外部AI APIは使わず、あなた自身が執筆します)。長考せず、要点を押さえて簡潔に執筆してください。

内容:
- 今週最重要ニュース(最大3件、各記事について「何が起きた?」「なぜ重要?実務ではどう使われる?」「初心者向け解説」「学習優先度」「公式ドキュメント」)
- 今週の技術トレンド(100〜200文字)
- 関連技術の列挙
- 今週の総括(200〜300文字)

ルール:
・記事単位ではなくテーマ単位で整理してください。
・同じ内容の記事は統合してください。
・一次情報を優先してください。
・図解(Mermaidなど)は一切含めないでください。
・Markdown形式で出力してください。

### 3. Notionに保存する
データソース collection://<BACKEND_WEEKLY_REPORTS_ID> (「Backend Weekly Reports」)に新規ページを作成してください。

- Name (title): "Backend Weekly Report {今日の日付 YYYY-MM-DD}"
- Date: 今日の日付
- Report Type: "Industry"
- Priority: レポート内容の重要度に応じて "High" / "Medium" / "Low" のいずれかをあなたが判断して設定(固定値にしない)
- Tags: レポートで扱った技術のうち、Tags列に現在すでに存在する選択肢の中からのみ該当するものを選んで設定してください。重要: notion-update-data-source ツールやその他の方法でTags列のスキーマ(選択肢)を変更しないでください。このタスクは無人・自動実行のため、スキーマ変更は承認待ちの権限プロンプトで処理が永久に停止する原因になります。該当する既存タグが1つもない場合は、Tagsを空のままにして処理を続けてください。
- content: 生成した「Backend Weekly Industry Report」のMarkdown全文をそのまま渡してください。

### 4. 完了報告
最後に、作成したNotionページのURL、使用できたソース数、かかったおおよその時間感を含めて簡潔に報告してください。

### 5-2. 日曜20:00 JST(毎週): Sunday Learning Planner

トリガー名の例: "Sunday Learning Planner"
cron: 毎週日曜 20:00(あなたのローカルタイムゾーンで設定してください)

プロンプト:

あなたは以下の自動化タスクを完全に自律的に実行してください。実行日時は毎週日曜20:00です。Notionツールを使用してください。

## 手順

### 1. 必要な情報をNotionから収集する

1. データソース collection://<BACKEND_WEEKLY_REPORTS_ID> (「Backend Weekly Reports」)から、Report Type = "Industry" で Date が最も新しいページを1件取得し、そのページ本文(Markdown)を取得する → weeklyReport とその日付(sourceReportDate)
2. データソース collection://<LEARNING_CONTEXTS_ID> (「Learning Contexts」)から、Status = "Active" のページを1件取得(なければ空)。あればそのページ本文(Markdown)を取得 → currentSprint。なければ currentSprint は空文字列。
3. データソース collection://<LEARNER_PROFILE_ID> (「Learner Profile」)から、Name = "Learner Profile" の行を1件取得し、そのプロパティ "Learner Profile" → learnerProfile、"Current Skills" → currentSkills、"Current Focus" → currentFocus を取得。
4. データソース collection://<AI_PROMPTS_ID> (「AI Prompts」)から、Name = "Sunday Learning Planner" かつ State = "Active" の行を1件取得し、プロパティ "System Prompt" → systemPrompt、"User Prompt Template" → userPromptTemplate を取得。

### 2. プロンプトを組み立てて自分自身で実行する
userPromptTemplate 内の以下のプレースホルダーを実際の値で置換してください。
- {{currentSprint}} → currentSprint
- {{currentFocus}} → currentFocus
- {{learnerProfile}} → learnerProfile
- {{currentSkills}} → currentSkills
- {{weeklyReport}} → weeklyReport
- {{reportDate}} → 本日の日付(YYYY-MM-DD)

置換後の文章を「ユーザーからのリクエスト」、systemPromptを「あなたの振る舞いのルール」として扱い、あなた自身が(外部AI APIを使わず)Sunday Learning Reportを生成してください。systemPrompt内のOutput Format指示に厳密に従い、Markdown形式で出力してください。

### 3. Notionに保存する
データソース collection://<SUNDAY_LEARNING_REPORTS_ID> (「Sunday Learning Reports」)に新規ページを作成してください。

- Title: "Sunday Learning Report {今日の日付 YYYY-MM-DD}"
- Date: 今日の日付
- Status: "Published"
- Version: "1.0.0"
- Source Report Date: 手順1で取得した元のBackend Weekly ReportのDate
- content: 生成したSunday Learning ReportのMarkdown全文

### 4. 学習チェックリストを作成する
手順3で生成したSunday Learning Reportの中から、今週の具体的な実践項目・タスク(例: 特定の技術を試す、特定の作業を行う、特定のドキュメントを読むなど、後で「終わった/終わっていない」を自分でチェックできる粒度のもの)を3〜6個程度抽出してください。

データソース collection://<LEARNING_CHECKLIST_ID> (「Learning Checklist」)に、抽出した項目それぞれについて新規ページを1件ずつ作成してください:
- Name (title): 項目の内容を簡潔に(1行、40文字程度を目安)
- Done (checkbox): false(未チェック)
- Reflected (checkbox): false(未チェック)
- Source Report Date (date): 今日の日付(手順3のSunday Learning Reportと同じ日付)

このステップで問題が起きても(データベースが見つからない等)、致命的なエラーとして扱わず、その旨を完了報告に含めた上で処理を継続してください。

### 5. 完了報告
最後に、作成したNotionページのURL(Sunday Learning Report本体、および学習チェックリストに追加した項目数)を含めて簡潔に完了を報告してください。もし元となるBackend Weekly Reportが見つからなかった場合は、無理に生成せず、その旨だけ報告して終了してください。

## Step 6: 確認・報告

すべて完了したら、以下を私に報告してください。

- Step 2で作成した6つのデータベースそれぞれのURL
- Step 5で作成した2つのスケジュールタスクの名前・次回実行予定時刻
- 手動でテスト実行できる場合は、土曜側のトリガーを一度手動実行して、Notionにページが正しく作成されるか確認し、結果を報告してください(その場合、Step 0は「未完了項目0件」で何もせずスキップされるのが正常です)
```

---

## Notes for whoever adapts this

- The template intentionally avoids the `" Version"` / `" Source Report Date"` leading-space property names that exist in the original author's live instance — those were an accidental naming quirk from the original manual setup, not something worth propagating.
- `Learning Completed` is added lazily by Step 0's sync logic the first time it actually has something to reflect, not during initial setup — this matches how the schema-change safety rule works in production (see [`../architecture/architecture.md`](../architecture/architecture.md), Unattended Safety).
- The RSS/search source list in the Saturday prompt is deliberately left open ("私が学びたい技術スタックに合わせて選んでください") rather than hardcoded to PHP/Laravel/AWS/Docker/Symfony, since a different learner will have a different stack.
