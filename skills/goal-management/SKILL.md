---
name: goal-management
description: "目標設定を支援。年間目標→半期→月次→週次の階層的な目標分解、弱み・課題の深掘り（AskUserQuestion使用）、前回評価B/C項目の自動抽出、評価項目との対応明記、会社の評価基準（Mission/Value、グレード制度）に基づいた目標設定をサポート。年間目標設定、半期目標設定、課題深掘り（壁打ち形式含む）のいずれかが必要な時に使用"
---

# 目標管理

会社の評価基準（3つのValue: Be hungry/Be an owner/Be sustainable）に基づいた階層的な目標設定（年間→半期→月次→週次）をサポート。

## ⚠️ 重要: 環境設定

**このスキルは特定の会社評価基準に基づいています。** 使用前に以下をカスタマイズしてください：

1. `~/.claude/skills/goal-management/references/grades.md` - 自社のグレード制度に変更
2. `~/.claude/skills/goal-management/references/values.md` - 自社の評価基準（Value定義）に変更

**初回使用時のチェック:**
- プロジェクトルートに `my-profile/` ディレクトリが存在しない場合、まず `profile-builder` スキルでプロフィールを作成してください
- 目標ファイルは自動的に `goals/` ディレクトリに保存されます

**会社の半期定義**:
- H1（前半期）: 3-8月（6ヶ月）
- H2（後半期）: 9-2月（6ヶ月）

## ワークフロー

### ステップ0: 年間目標設定（初回または年度開始時）

`goals/YYYY-年間目標.md` に年間目標が存在するか確認:
- 存在しない → ユーザーに年間目標の作成を促す
- 存在する → ステップ1へ進む

**年間目標のプロセス**:
1. 年間テーマの設定
2. Value別の目標設定（Be hungry/Be an owner/Be sustainable）
3. KPIの設定
4. 2つの半期目標への分解
5. `goals/YYYY-年間目標.md` に保存

**詳細**: [annual-goal-framework.md](references/annual-goal-framework.md)

### ステップ1: プロフィール確認

プロジェクトルートに `my-profile/` が存在するか確認:
- 存在する → プロフィール情報（経歴、役割、強み・弱み、キャリア目標）を読み込む
- 存在しない → まず profile-builder skill でプロフィールを作成するようユーザーに促す

### ステップ2: 評価基準の確認

1. グレード要件を読み込む: [grades.md](references/grades.md)
2. Value評価基準を読み込む: [values.md](references/values.md)
3. 前回評価のCSVが存在する場合は読み込む（B/C項目を抽出）

### ステップ3: 課題の深掘り（重要）

目標を提案する前に、課題を深掘りする:

1. 前回評価からB/C項目を自動抽出
2. AskUserQuestionで現在の課題を特定
3. 5 Whysで深掘り（各回答に対してAskUserQuestionを使用）
4. 改善アクションをブレインストーミング
5. 課題を3つのValueに分類

**詳細**: [challenge-deepdive-questions.md](references/challenge-deepdive-questions.md)

### ステップ4: 半期目標の提案

評価項目とのマッピングを含む3つのValue別の目標を提案:

**Be hungry（貪欲になれ）**:
- 項目1: 貪欲に新しいものを勉強したか
- 項目2: 貪欲に仕事に取り組んだか

**Be an owner（当事者たれ）**:
- 項目1: 締め切り意識を持てたか
- 項目2: チーム全体の生産性に寄与したか

**Be sustainable（継続的であれ）**:
- 項目1: 属人化しないように仕組み化するようになにかやれたか
- 項目2: 日常業務に対して改善する動きができたか

各目標は以下を満たす必要がある:
- 特定の評価項目にマッピング
- トラッキング用に `[ ]` チェックボックス形式を使用
- SMART（Specific, Measurable, Achievable, Relevant, Time-bound）である

**フォーマット詳細**: [goal-format.md](references/goal-format.md)

### ステップ5: 自動分解（必須）

半期目標を提案した後、自動的に分解:

1. 月次目標（全6ヶ月分）
   - 月ごとの具体的なアクション
   - マイルストーン

2. 週次目標（全約24週分）
   - 各月を4-5週に分解
   - 週ごとの具体的なタスク

3. プロジェクトルートの `goals/` に保存:
   - `YYYY-年間目標.md`（年間目標が存在する場合）
   - `YYYY-HX-半期目標.md`
   - `YYYY-HX-月次目標.md`（全6ヶ月を1ファイルに）
   - `YYYY-HX-週次目標.md`（全約24週を1ファイルに）

**重要**: プロジェクトルートの `goals/` に保存すること。`.claude/skills/goal-management/goals/` ではない。

### ステップ6: レビューと調整

1. 生成した目標をユーザーに提示
2. 調整が必要か確認
3. 必要に応じて修正

## 目標調整パターン

ユーザーが目標を調整したい場合:

**a) 目標が高すぎる**
- 「記事12本 → 記事6本」
- 数値を調整し、月次・週次を再計算

**b) 目標を追加**
- 適切なValueに追加
- `[ ]` 形式を使用
- 評価項目にマッピング

**c) 目標を削除**
- 半期目標から削除
- 月次・週次からも削除

**d) 評価項目とのマッピングが不明確**
- どの評価項目にマッピングされるか明確にする

## 重要な原則

1. **事実ベース**: すべての評価は具体的な事実・エピソードに基づく（ログから自動抽出可能）
2. **3つのValue**: 常に Be hungry/Be an owner/Be sustainable を考慮
3. **評価項目とのマッピング（必須）**: 各目標は評価項目にマッピングする必要がある
4. **グレード要件**: 現在のグレードの期待値を明確に理解
5. **階層的**: 達成可能性のため半期→月次→週次に分解

## 評価ランク

- **S**（期待を大きく上回る）: 給与アップ
- **A**（期待をやや上回る）: 給与アップ
- **B**（期待通り）: 給与据え置き
- **C**（期待に達していない）: 前回C以下の場合、給与ダウン
- **D**（できているとはいえない）: 給与ダウン

## リファレンス

- [annual-goal-framework.md](references/annual-goal-framework.md) - 年間目標フレームワーク
- [challenge-deepdive-questions.md](references/challenge-deepdive-questions.md) - 課題深掘り質問
- [grades.md](references/grades.md) - グレード制度詳細
- [values.md](references/values.md) - Value評価基準
- [templates.md](references/templates.md) - 評価シートテンプレート
- [goal-format.md](references/goal-format.md) - 目標フォーマット例
- [examples.md](references/examples.md) - 使用例
