# 変数定義マスタ

Config Agent がチャット UI でユーザーから収集する変数と、収集した値から自動導出する変数を定義します。

---

## 変数の分類

| 分類 | 説明 | 対応 |
|------|------|------|
| **CORE** | ユーザーが必ず答える（13問） | チャット UI で質問 |
| **DERIVED** | CORE 変数から Config Agent が自動生成 | AI が導出・ユーザー任意修正可 |

---

## CORE 変数（チャットで収集・13問）

### グループ 1：基本情報

| 変数名 | UI 質問文 | 入力形式 | 記入例 |
|--------|----------|---------|--------|
| `SERIES_NAME` | このシリーズの名前を教えてください | 自由記述 | AI経営実装シリーズ |
| `EPISODE_COUNT` | 全何回のシリーズですか？ | 数値 | 10 |
| `TOPIC_DOMAIN` | 記事のテーマ・ドメインを一言で教えてください | 自由記述 | AI組織実装・AI経営実装 |

### グループ 2：ターゲット設定

| 変数名 | UI 質問文 | 入力形式 | 記入例 |
|--------|----------|---------|--------|
| `AUTHOR_PERSONA` | 記事を書く「著者」のペルソナを教えてください | 自由記述 | BtoBコンサルティング会社の編集責任者兼、経営層向けコンテンツマーケティングの専門家 |
| `TARGET_READERS` | 対象読者を具体的な役職・職種で列挙してください | カンマ区切り | CEO、COO、CFO、DX担当役員、経営企画、事業部長 |
| `TARGET_READER_GROUP` | 上記読者の総称を一言で教えてください | 自由記述（短め） | 経営層 |

### グループ 3：目的・ゴール

| 変数名 | UI 質問文 | 入力形式 | 記入例 |
|--------|----------|---------|--------|
| `ARTICLE_GOAL` | 記事を読んだ読者に最終的に取ってほしいアクションを教えてください | 自由記述 | AI組織実装・AI経営実装に関するコンサルティングサービスの相談・受注につなげること |
| `SERVICE_NAME` | 記事で訴求するサービスまたは商品の名称を教えてください | 自由記述 | AI経営実装コンサルティング |
| `CORE_PROBLEM` | 読者が抱える核心的な問題・痛みを一言で表してください | 自由記述 | 使っているのに成果が出ない |

### グループ 4：コンテンツ設計

| 変数名 | UI 質問文 | 入力形式 | 記入例 |
|--------|----------|---------|--------|
| `FRAMEWORK_EXAMPLES` | 記事に入れる独自フレームワーク・診断軸の例を教えてください（複数可） | 箇条書き | AI成果化成熟度診断 / AI PoC学習成熟度診断 / 90日実装ロードマップ |
| `BUSINESS_FUNCTIONS` | 具体例として登場させる業務機能・部門を列挙してください | カンマ区切り | 営業、契約、法務、経理、人事、問い合わせ対応、経営企画 |
| `TRUSTED_SOURCES` | 引用する信頼できる情報源を列挙してください | カンマ区切り | BCG、PwC、JUAS、NRI、企業公式発表 |

### グループ 5：CTA 設計

| 変数名 | UI 質問文 | 入力形式 | 記入例 |
|--------|----------|---------|--------|
| `CTA_DIAGNOSIS_ITEMS` | 初回相談・診断で読者に提供できる価値を具体的に列挙してください | 箇条書き | 現在のAI活用状況の棚卸し / 成果につながっていない要因特定 / 優先的に実装すべき業務 / 経営会議で追うべきKPI / 次の90日で着手すべきアクション |

---

## DERIVED 変数（Config Agent が自動導出）

収集した CORE 変数をもとに、Config Agent が以下を自動生成します。
ユーザーは確認画面で任意修正できます。

### 導出ルール

| 変数名 | 導出ロジック | 導出例（AI経営実装） |
|--------|------------|-------------------|
| `READER_FEELING_1` | `CORE_PROBLEM` から「自社課題の認識」を生成 | これは自社の経営課題だ |
| `READER_FEELING_2` | `CORE_PROBLEM` + `ARTICLE_GOAL` から「危機感」を生成 | このまま放置するとAI投資が成果化しない |
| `READER_FEELING_3` | `ARTICLE_GOAL` から「行動意欲」を生成 | 一度、自社の状況を診断してもらう必要がある |
| `CORE_QUALITY_QUESTION` | `TOPIC_DOMAIN` + `ARTICLE_GOAL` から記事の中心的な問いを生成 | AI活用の問題を、経営成果・投資判断・組織設計・業務プロセス・KPI・ガバナンスの問題として描けているか |
| `DISQUALIFIED_PATTERN_1` | `TOPIC_DOMAIN` から「ツール紹介に寄りすぎ」パターンを生成 | AIツール紹介で終わっている |
| `DISQUALIFIED_PATTERN_2` | `TOPIC_DOMAIN` から「ノウハウ紹介」パターンを生成 | プロンプト術や現場活用ノウハウで終わっている |
| `DISQUALIFIED_PATTERN_3` | `TOPIC_DOMAIN` から「一般論」パターンを生成 | 「AIで効率化できます」という一般論で終わっている |
| `DISQUALIFIED_PATTERN_4` | `TARGET_READER_GROUP` から「読者視点」パターンを生成 | 経営層が自社の投資判断・組織課題として読めない |
| `OPENING_HOOK_EXAMPLE` | `CORE_PROBLEM` から冒頭フックの例を生成 | 「AIを使うかどうか」ではなく、「AI投資が成果に変わっているか」を問う |
| `READER_ACCOUNTABILITY` | `TARGET_READER_GROUP` から説明責任先を生成 | 取締役会、経営会議、投資家、株主への説明責任 |
| `STRUCTURAL_DIMENSIONS` | `TOPIC_DOMAIN` から構造的問題の側面を生成 | 業務プロセス、KPI、責任範囲、ガバナンス、組織学習 |
| `IMPACT_LIST` | `TOPIC_DOMAIN` + `ARTICLE_GOAL` からインパクト例を生成 | AI投資ROI / 重複投資 / 機会損失 / ガバナンス不全 / 競争優位の喪失 |
| `IMPACT_PERSPECTIVE_1` | `ARTICLE_GOAL` から投資・成果系の観点を生成 | 投資判断・経営成果・組織設計・リスク・競争優位 |
| `TITLE_KEYWORDS` | `TOPIC_DOMAIN` + `ARTICLE_GOAL` からタイトルキーワードを生成 | 経営課題、投資対効果、成果化、組織設計、ガバナンス |
| `PAIN_POINT_EXAMPLES` | `CORE_PROBLEM` から複数の痛み例を生成 | 「使っているのに成果が出ない」「PoCをしているのに横展開されない」「現場任せで経営成果が見えない」 |
| `MGMT_TOPIC_AREA` | `ARTICLE_GOAL` から経営課題領域を生成 | 投資、ROI、機会損失、ガバナンス、説明責任 |
| `ACTION_ITEMS` | `TOPIC_DOMAIN` + `TARGET_READER_GROUP` から取るべきアクションを生成 | 経営会議で見るべき指標 / 優先順位の決め方 / ガバナンス設計 / 90日以内の実装ステップ |
| `SUMMARY_REFRAME` | `TOPIC_DOMAIN` から問題の再定義文を生成 | 「AIの問題」ではなく「経営設計の問題」 |
| `SCORING_ITEM_3_LABEL` | `"{{TARGET_READER_GROUP}}への刺さり"` で固定生成 | 経営層への刺さり |
| `SCORING_ITEM_3_DESC` | `TARGET_READER_GROUP` + `TARGET_READERS` から説明文を生成 | CEO、COO、CFO、DX担当役員が自分ごととして読めるか |
| `SCORING_ITEM_3_SUBPOINT` | `READER_ACCOUNTABILITY` から補足説明を生成 | 取締役会、経営会議、投資家、株主への説明責任に接続できているか |
| `SCORING_ITEM_5_LABEL` | `"ビジネスインパクト"` で固定（またはドメイン依存で調整） | 経営インパクト |
| `SCORING_ITEM_3_10PT` | `TARGET_READERS` から10点基準を生成 | CEO、COO、CFO、DX役員それぞれが読む理由がある |
| `SCORING_ITEM_3_7PT` | `TARGET_READERS` から7点基準を生成 | DX担当役員には刺さるが、CEO/CFOにはやや弱い |
| `SCORING_ITEM_3_4PT` | `TARGET_READER_GROUP` から4点基準を生成 | 部長・担当者向けに見える |
| `SCORING_ITEM_5_10PT` | `IMPACT_LIST` から10点基準を生成 | ROI、機会損失、重複投資、ガバナンス、競争優位に接続している |
| `SCORING_ITEM_5_7PT` | `"一部接続している"` で固定 | 一部接続している |
| `SCORING_ITEM_5_4PT` | `"業務効率化止まり"` で固定 | 業務効率化止まり |
| `SCORING_ITEM_9_LABEL` | `"受注・行動導線"` で固定 | 受注導線 |
| `SCORING_ITEM_9_DESC` | `ARTICLE_GOAL` から導線説明を生成 | 最後に自然な相談・行動導線があるか |
| `DEDUCTION_PATTERN_1` | `TOPIC_DOMAIN` から減点パターン①を生成 | AIツール紹介に寄りすぎている（-10点） |
| `DEDUCTION_PATTERN_2` | `TOPIC_DOMAIN` から減点パターン②を生成 | プロンプト術の記事になっている（-10点） |
| `DEDUCTION_PATTERN_3` | `TARGET_READER_GROUP` から減点パターン③を生成 | 経営層ではなく担当者向けに見える（-10点） |
| `ADDITION_PATTERN_1` | `FRAMEWORK_EXAMPLES` から加点パターン①を生成 | 独自診断軸がCTAと接続している（+5点） |
| `ADDITION_PATTERN_2` | `CTA_DIAGNOSIS_ITEMS` から加点パターン②を生成 | 初回診断で得られる成果物が具体的（+5点） |
| `CONSECUTIVE_TOPIC` | `"第N回記事として、次回以降への接続"` で固定 | 第N回記事として、次回以降への接続が自然か |
| `CTA_BAD_EXAMPLE` | `"お気軽にお問い合わせください"` で固定 | お気軽にお問い合わせください |
| `CTA_GOOD_EXAMPLE` | `CTA_DIAGNOSIS_ITEMS` から良いCTA文を自動生成 | 初回診断では、現在のAI活用状況を棚卸しし… |

---

## 変数の使用先マッピング

| 変数名 | input | prompt_記事作成 | prompt_品質チェック | output_記事構造 |
|--------|:-----:|:---------------:|:-----------------:|:---------------:|
| SERIES_NAME | ✓ | ー | ー | ✓ |
| EPISODE_COUNT | ✓ | ー | ー | ー |
| TOPIC_DOMAIN | ー | ✓ | ✓ | ✓ |
| AUTHOR_PERSONA | ー | ✓ | ✓ | ー |
| TARGET_READERS | ー | ✓ | ✓ | ✓ |
| TARGET_READER_GROUP | ✓ | ✓ | ✓ | ✓ |
| ARTICLE_GOAL | ー | ✓ | ✓ | ー |
| SERVICE_NAME | ー | ✓ | ー | ー |
| CORE_PROBLEM | ー | ✓ | ✓ | ー |
| FRAMEWORK_EXAMPLES | ー | ✓ | ー | ✓ |
| BUSINESS_FUNCTIONS | ー | ✓ | ✓ | ✓ |
| TRUSTED_SOURCES | ー | ✓ | ✓ | ✓ |
| CTA_DIAGNOSIS_ITEMS | ー | ✓ | ✓ | ✓ |
| （DERIVED 変数） | ー | ✓ | ✓ | ー |

---

## 収集セッションの状態管理

Config Agent は収集した変数を `runtime/variables.json` に保存します。

```json
{
  "project_id": "ai_keiei_series",
  "created_at": "2026-05-09T10:00:00Z",
  "core": {
    "SERIES_NAME": "AI経営実装シリーズ",
    "EPISODE_COUNT": "10",
    "TOPIC_DOMAIN": "AI組織実装・AI経営実装",
    "AUTHOR_PERSONA": "BtoBコンサルティング会社の編集責任者兼、経営層向けコンテンツマーケティングの専門家",
    "TARGET_READERS": "CEO、COO、CFO、DX担当役員、経営企画、事業部長",
    "TARGET_READER_GROUP": "経営層",
    "ARTICLE_GOAL": "AI組織実装・AI経営実装に関するコンサルティングサービスの相談・受注につなげること",
    "SERVICE_NAME": "AI経営実装コンサルティング",
    "CORE_PROBLEM": "使っているのに成果が出ない",
    "FRAMEWORK_EXAMPLES": "AI成果化成熟度診断 / AI PoC学習成熟度診断 / 90日実装ロードマップ",
    "BUSINESS_FUNCTIONS": "営業、契約、法務、経理、人事、問い合わせ対応、経営企画",
    "TRUSTED_SOURCES": "BCG、PwC、JUAS、NRI、企業公式発表",
    "CTA_DIAGNOSIS_ITEMS": "現在のAI活用状況の棚卸し / 成果につながっていない要因特定 / 優先的に実装すべき業務 / 経営会議で追うべきKPI / 次の90日で着手すべきアクション"
  },
  "derived": {
    "READER_FEELING_1": "これは自社の経営課題だ",
    "CTA_GOOD_EXAMPLE": "初回診断では、現在のAI活用状況を棚卸しし…",
    "..."
  }
}
```
