# Skills

開発ワークフローと学習を効率化するClaude Skills集

## 🚀 インストール

全てのスキルを一括インストール：

```bash
/plugin install okazuki-skills@okazuki-skills
```

または対話的なUIで選択：

```bash
/plugin
```

## ⚙️ 前提条件・セットアップ

### 共通の必要ツール

一部のスキルは以下のツールを必要とします：

- **GitHub CLI (`gh`)**: learning-support, task-management で必要
  ```bash
  # インストール（macOS）
  brew install gh

  # 認証
  gh auth login
  ```

### スキル別の前提条件と適用範囲

| スキル | 適用範囲 | 必要な準備 | 制約・注意事項 |
|--------|---------|-----------|--------------|
| **profile-builder** | ✅ 汎用的 | なし | プロジェクトルートに `my-profile/` を作成 |
| **spec-to-code** | ✅ 汎用的 | なし | 言語・フレームワーク非依存 |
| **learning-support** | ✅ 汎用的 | GitHub CLI | GitHubアカウントが必要 |
| **task-management** | ✅ 汎用的 | GitHub CLI | GitHubリポジトリが必要 |
| **goal-management** | ⚠️ 特定環境向け | profile-builder実行 | 特定の会社評価基準に依存（要カスタマイズ） |
| **maclogger-review** | ⚠️ 特定環境向け | goal-management実行 | 特定のパス構造に依存（要カスタマイズ） |

### カスタマイズが必要なスキル

以下のスキルは作者の環境に特化しています。使用前にカスタマイズが必要です：

#### goal-management

特定の会社の評価基準（Mission/Value、グレード制度）に基づいています。
自社の評価基準に合わせて以下のファイルを編集してください：

- `skills/goal-management/references/grades.md` - グレード制度
- `skills/goal-management/references/values.md` - 評価基準（Value定義）

#### maclogger-review

出力先とログ読み取り先のパスがハードコードされています：
- 出力先: `/Users/ogawakazuki/dev/xincere-review/reviews/`
- ログ元: `/Users/ogawakazuki/dev/maclogger/reports/`

**カスタマイズ方法:**
1. `~/.claude/skills/maclogger-review/SKILL.md` を開く
2. 以下の行を検索して自分のパスに変更:
   - `約30-50行のマークダウンとして /Users/ogawakazuki/dev/xincere-review/reviews/ に出力`
   - `**週次**: /Users/ogawakazuki/dev/xincere-review/reviews/weekly/YYYY-WXX.md`
   - `**月次**: /Users/ogawakazuki/dev/xincere-review/reviews/monthly/YYYY-MM.md`
   - `**半期**: /Users/ogawakazuki/dev/xincere-review/reviews/YYYY-HX.md`
   - `**日次ログ**: /Users/ogawakazuki/dev/maclogger/reports/daily/YYYY-MM-DD.md`
   - `**週次ログ**: /Users/ogawakazuki/dev/maclogger/reports/weekly/YYYY-WXX.md`

または、このスキルを使わずに汎用的な振り返りには他のスキルを使用することをお勧めします。

## 🚦 Quick Start

初めて使う場合は、以下の順番で進めることをお勧めします：

### 汎用的なスキルから始める

1. **新しい技術を学びたい**
   ```
   Dockerを勉強したい
   ```
   → learning-support が自動起動

2. **プロジェクトのタスク管理をしたい**
   ```
   このリポジトリのタスク管理をしたい
   ```
   → task-management が自動起動

3. **新機能の仕様を明確にしたい**
   ```
   @spec_template.md を読んで、カート機能についてインタビューしてください
   ```
   → spec-to-code が自動起動

### 目標管理スキルを使う（カスタマイズが必要）

特定環境向けスキルを使う場合：

1. **profile-builder でプロフィール作成**
   ```
   プロフィールを作成して
   ```
   → `my-profile/` が作成される

2. **goal-management と maclogger-review をカスタマイズ**（上記参照）

3. **目標設定・振り返りを開始**
   ```
   今期の目標を立てたい
   先週の振り返りを作成して
   ```

## 📁 利用可能なSkills

### 🎯 目標管理・振り返り系

#### goal-management

**目標設定を支援するスキル**

年間目標→半期→月次→週次の階層的な目標分解、弱み・課題の深掘り（AskUserQuestion使用）、前回評価B/C項目の自動抽出、maclogger連携をサポート。

**使用シーン:**
- 年間目標設定時
- 半期目標設定時
- 課題深掘り（壁打ち形式）

**使用例:**
```
今期の目標を立てたい
2025年の年間目標を作成したい
```

#### maclogger-review

**週次目標の達成状況を確認し、次週へのアクションを明確にする振り返りスキル**

週次・月次・半期の振り返り作成、AskUserQuestionで対話的に振り返りを進める壁打ち形式をサポート。

**使用シーン:**
- 週次振り返り作成時
- 月次振り返り作成時
- 半期振り返り作成時
- うまくいかなかったことの整理

**使用例:**
```
先週の振り返りを作成して
今週うまくいかなかったことがあるので、一緒に振り返りたい
```

#### profile-builder

**10分で完成する簡潔なプロフィール作成スキル**

経験年数・年収・技術理解度チェックから一般的な課題を自動生成。ジュニア/ミドル/シニアのレベルに応じた最適なプロフィールを自動生成。

**使用シーン:**
- 初回プロフィール作成時
- 半期ごとのプロフィール更新時

**使用例:**
```
プロフィールを作成して
```

### 📚 学習サポート系

#### learning-support

**包括的な学習サポートシステム**

「〇〇を勉強したい」と言うだけで、理解度チェック→学習計画立案→リポジトリ作成→GitHub Projects作成→Issue作成→伴走サポートまで自動化。あらゆるトピック（Ruby、AWS、Docker、アルゴリズムなど）に対応。

**使用シーン:**
- 新しい技術を学習したい時
- 体系的に学習を進めたい時
- 学習進捗を管理したい時

**使用例:**
```
Dockerを勉強したい
AWSを学習したい
アルゴリズムとデータ構造を学びたい
```

### 🛠️ 開発・タスク管理系

#### task-management

**リポジトリベースのタスク管理支援システム**

リポジトリ内容を分析→対話的に計画立案→タスク分解（大→中→小）→GitHub Projects作成→Issue作成（優先順位・ラベル・マイルストーン付き）まで自動化。

**使用シーン:**
- プロジェクトのタスク管理をしたい時
- 既存プロジェクトのタスクを整理したい時
- GitHub ProjectsとIssueを一括作成したい時

**使用例:**
```
このリポジトリのタスク管理をしたい
プロジェクトの計画を立てたい
```

#### spec-to-code

**仕様駆動・テスト駆動開発ワークフロー**

仕様策定から実装までの開発ワークフローをガイド。仕様インタビュー→ドキュメント化→GitHub Issue生成→TDD実践を支援。

**使用シーン:**
- 新機能の開発開始時
- 要件が曖昧で明確化が必要な時
- テスト駆動開発（TDD）を実践したい時
- 構造化されたGitHub Issueを作成したい時

**使用例:**
```
@spec_template.md を読んで、カート機能についてインタビューしてください
```

## 🌟 特徴

- **目標管理**: 年間→半期→月次→週次の階層的な目標分解、macloggerと連携した振り返り
- **学習サポート**: GitHub Issue駆動の体系的な学習、小ステップ学習で確実に理解
- **タスク管理**: リポジトリベースのタスク管理、GitHub ProjectsとIssueの自動作成
- **仕様駆動開発**: 仕様の明確化、TDDのベストプラクティス
- **言語・フレームワーク非依存**: どんな技術スタックでも使える
- **対話的なサポート**: AskUserQuestionで対話しながら最適な計画を立案

## 🔗 スキル間の連携

- **profile-builder → goal-management**: プロフィール作成後、目標設定に活用
- **goal-management → maclogger-review**: 目標設定後、週次で振り返り
- **learning-support → task-management**: 学習した技術を使ってプロジェクトを進める
- **goal-management → learning-support**: 目標（Be hungry - 新技術学習）を学習タスク化
- **task-management → goal-management**: プロジェクト進捗を目標達成の証跡として記録

## 📜 ライセンス

MIT License
