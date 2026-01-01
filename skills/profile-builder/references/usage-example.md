## 使用例

```
ユーザー: profile-builder skillを使ってプロフィールを作成して

Claude: プロフィールを作成します。

【所要時間】: 10分程度（全10問）
【形式】: 選択式

既存のプロフィールを確認します...

（既存プロフィールがない場合）
既存のプロフィールはありませんので、新規作成します。

---

Q1/10. エンジニア歴を選んでください
1. 1年未満
2. 1-3年（ジュニアレベル）
3. 3-5年（ミドルレベル）
...
> 2

Q2/10. 現在の年収レンジを選んでください
1. 300万未満
2. 300-400万（ジュニアレベル）
...
> 2

Q3/10. 現在のグレードを選んでください
1. G1 - 見習い
2. G2 - 作業者
...
> 2

（以下、Q4-Q10の質問が続く）

---

Claude: プロフィールを作成しました！

【判定結果】
- レベル: ジュニアエンジニア
- 経験: 1-3年
- 年収: 300-400万
- グレード: G2

【生成されたファイル】
- my-profile/SKILL.md: メインプロフィール
- my-profile/references/background.md: 経歴・スキルセット
- my-profile/references/current-role.md: 現在の業務・役割
- my-profile/references/strengths-weaknesses.md: 強み・課題
- my-profile/references/career-goals.md: キャリア目標

次は、goal-management skillを使って半期目標を立てることができます。
```
