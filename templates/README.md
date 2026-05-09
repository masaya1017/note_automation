# テンプレートフォルダ

本フォルダは、note記事自動作成システムの**雛形セット**です。
ユーザーはファイルを直接編集しません。チャット UI で変数を答えると、Config Agent が自動的にテンプレートへ埋め込みます。

---

## フォルダ構成

```
templates/
├── README.md                        ← 本ファイル
├── variables_definition.md          ← 変数定義マスタ（Config Agent が参照）
├── input_記事構成_template.md       ← 記事シリーズ構成表の雛形
├── prompt_記事作成_template.md      ← Writing Agent 用プロンプトの雛形（13 CORE + 34 DERIVED 変数）
├── prompt_品質チェック_template.md  ← Quality Check Agent 用プロンプトの雛形
└── output_記事構造_template.md      ← 完成記事の9章構成・文体基準の雛形
```

---

## ユーザーの操作フロー

```
1. チャットに「記事を作成したい」と入力

2. Config Agent が 5グループ・13問の質問を順番に提示
   ├─ グループ1: 基本情報（シリーズ名・テーマ・回数）
   ├─ グループ2: ターゲット（著者ペルソナ・対象読者）
   ├─ グループ3: 目的・ゴール（記事目的・サービス名・中心問題）
   ├─ グループ4: コンテンツ設計（フレームワーク・業務機能・情報源）
   └─ グループ5: CTA（初回診断で提供する価値）

3. Config Agent が残り約34変数を自動導出し、設定サマリーを提示
   └─ ユーザーは「確認 / 修正」を選択

4. Config Agent がテンプレートに変数を埋め込み、runtime/ に保存
   ├─ runtime/variables.json
   ├─ runtime/input/記事構成.md
   ├─ runtime/prompt/記事作成プロンプト.md
   ├─ runtime/prompt/品質チェックプロンプト.md
   └─ runtime/output/記事構造.md

5. マルチエージェントシステム（MAS）が runtime/ を参照して起動
```

---

## 変数の分類

| 分類 | 数 | 誰が入力 | 詳細 |
|------|----|---------|----- |
| **CORE** | 13変数 | ユーザー（チャット） | `variables_definition.md` の CORE 変数参照 |
| **DERIVED** | 約34変数 | Config Agent（AI自動導出） | `variables_definition.md` の導出ルール参照 |

---

## このフォルダを直接操作する場面

- **新テーマへの転用**: `variables_definition.md` を参考に別プロジェクトの変数を確認したいとき
- **テンプレート自体の改修**: 雛形の構成・評価基準を変えたいとき
- **デバッグ**: `runtime/variables.json` と照らし合わせて変数の導出結果を確認したいとき

通常の記事作成では、このフォルダを直接触る必要はありません。
