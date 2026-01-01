# 学習サポート - 使用例

## 例1: Docker学習

```
User: "Dockerを勉強したい"

Claude:
1. [理解度チェック]
   Q1: "Dockerについて、現在どの程度理解していますか？"
   → User: "まったく知らない"

   Q2: "Dockerを学ぶ目的は何ですか？"
   → User: "実務で使えるようになりたい"

   Q3: "どのくらいの期間で学習を完了したいですか？"
   → User: "1週間"

2. [学習計画立案]
   "Dockerの学習計画を立てました：

   Day 1: Dockerの基礎概念とインストール
   Day 2: Dockerイメージの作成とDockerfile
   Day 3: コンテナの操作とデータ永続化
   Day 4: Docker Composeによる複数コンテナ管理
   Day 5: 実践演習とベストプラクティス

   この計画で進めてよろしいですか？"
   → User: "はい"

3. [リポジトリ作成]
   "リポジトリ名は `docker-learning` でよろしいですか？"
   → User: "はい"

   "リポジトリの可視性は？ Public / Private"
   → User: "Public"

   [実行: gh repo create]
   [README.md 作成]

4. [GitHub Projects 作成]
   [実行: gh project create]

5. [Issue 作成]
   [5つのIssueを作成]
   [README.mdにIssue番号を追加]

   "学習環境が整いました！
   リポジトリ: https://github.com/{owner}/docker-learning
   Projects: https://github.com/{owner}/docker-learning/projects/1

   準備ができたら「Day 1を学習したい」と言ってください。"

---

User: "Day 1を学習したい"

Claude:
1. [Issue #1を読み込み]
2. [5-7ステップに分解]
   "Day 1を以下のステップで学習します：

   Step 1: Dockerとは何か、コンテナとVMの違い
   Step 2: Dockerのインストール（Mac/Windows/Linux）
   Step 3: hello-worldコンテナの実行
   Step 4: docker run コマンドの基本
   Step 5: コンテナのライフサイクル

   このステップで進めていきましょうか？"
   → User: "はい"

3. [Step 1 開始]
   "では Step 1 から始めます。

   【Dockerとは】
   Dockerは、アプリケーションとその依存関係を「コンテナ」という軽量な環境にパッケージ化する技術です。

   コンテナ vs VM:
   - VM: OS全体を仮想化（重い）
   - コンテナ: アプリケーションレイヤーのみを仮想化（軽い）

   【演習】
   以下の質問に答えてください：
   1. なぜDockerはVMより軽量なのか？
   2. Dockerを使うメリットは何か？（3つ挙げてください）"

   [ユーザーが回答]

   [コードレビュー]
   [「なぜ」の説明を含む解答]
   [良い理解 vs 誤解の例]

   "この概念は理解できましたか？"
   → User: "はい"

4. [Step 2に続く]
   ...

---

[すべてのステップ完了後]

User: "終わり"

Claude:
1. [サマリー生成]
2. [Issue #1に投稿]
3. [Issue #1をクローズ]
4. [Projectsボードを更新]

"お疲れ様でした！Day 1の学習内容を Issue #1 にまとめました。
次は Day 2 に進みますか？"
```

## 例2: AWS学習

```
User: "AWSを勉強したい"

Claude:
[理解度チェック → 学習計画立案]

"AWSの学習計画を立てました：

Day 1: AWSの基礎概念とアカウント作成
Day 2: EC2（仮想サーバー）の基本
Day 3: S3（ストレージ）の基本
Day 4: RDS（データベース）の基本
Day 5: VPCとセキュリティグループ
Day 6: IAM（アクセス管理）
Day 7: 実践演習（簡単なWebアプリのデプロイ）

この計画で進めてよろしいですか？"

[Docker例と同じワークフローで続行]
```

## 例3: 学習計画の分解（Dockerの場合）

```
Step 1 (Day 1): Dockerの基礎概念とインストール
  - 学習目標: Dockerとは何か、コンテナとVMの違いを理解
  - 成果物: Docker環境構築、hello-worldコンテナ実行
  - 所要時間: 2-3時間

Step 2 (Day 2): Dockerイメージの作成とDockerfile
  - 学習目標: Dockerfileの書き方、イメージのビルド方法
  - 成果物: 簡単なWebアプリのDockerイメージ作成
  - 所要時間: 3-4時間

Step 3 (Day 3): コンテナの操作とデータ永続化
  - 学習目標: コンテナの起動・停止・削除、Volumeの使い方
  - 成果物: データベースコンテナの作成
  - 所要時間: 3-4時間

Step 4 (Day 4): Docker Composeによる複数コンテナ管理
  - 学習目標: docker-compose.ymlの書き方、複数コンテナの連携
  - 成果物: Web + DB の構成を作成
  - 所要時間: 3-4時間

Step 5 (Day 5): 実践演習とベストプラクティス
  - 学習目標: Dockerを使った開発環境構築
  - 成果物: 実際のプロジェクトをDocker化
  - 所要時間: 4-5時間
```
