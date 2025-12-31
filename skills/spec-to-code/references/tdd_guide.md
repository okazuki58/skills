# Test-Driven Development (TDD) Guide

このガイドはテスト駆動開発の実践方法を定義します。言語・フレームワーク非依存です。

---

## TDDの基本サイクル

```
1. RED: 失敗するテストを書く
   ↓
2. GREEN: テストを通す最小限の実装
   ↓
3. REFACTOR: コードを整理
   ↓
  (次の機能へ)
```

---

## 想定される使い方

### 1. 仕様からテストを作成

```
Issue #1のテストを書いてください。

対象機能: [機能名]
テストファイル: tests/feature_test.{拡張子}
```

Claudeが:
- Issueの受け入れ基準を読む
- テストシナリオを実装
- 失敗することを確認（RED状態）

### 2. テストを通す実装

```
{テストファイル名} のテストが通るように、
@tdd_guide.md を参考に実装してください。
```

Claudeが:
1. テストを読んで要件を理解
2. 最小限の実装でテストを通す（GREEN）
3. コードを整理（REFACTOR）

---

## テストの書き方

### 単体テスト

**最小単位の機能をテスト**

```python
# Python example
def test_add_to_cart():
    cart = Cart()
    product = Product(id=1, name="Book", price=1000)
    
    cart.add(product)
    
    assert cart.item_count() == 1
    assert cart.total() == 1000
```

```javascript
// JavaScript example
test('adds product to cart', () => {
  const cart = new Cart();
  const product = { id: 1, name: 'Book', price: 1000 };
  
  cart.add(product);
  
  expect(cart.itemCount()).toBe(1);
  expect(cart.total()).toBe(1000);
});
```

### 統合テスト

**複数コンポーネントの連携をテスト**

```python
# Python + Selenium example
def test_user_can_add_to_cart():
    browser.visit('/products/1')
    browser.click('Add to Cart')
    
    assert browser.find('cart-count').text == '1'
    assert browser.current_url == '/cart'
```

```javascript
// JavaScript + Playwright example
test('user can add to cart', async ({ page }) => {
  await page.goto('/products/1');
  await page.click('text=Add to Cart');
  
  await expect(page.locator('#cart-count')).toHaveText('1');
  await expect(page).toHaveURL('/cart');
});
```

---

## TDD実装フロー

### Step 1: RED（失敗するテストを書く）

```
1. Issueの受け入れ基準を読む
2. 最も小さい単位のテストから書く
3. テストを実行して失敗を確認
```

**ポイント**: 
- まだ機能がないので、当然テストは失敗する
- これが正しい状態（RED）

### Step 2: GREEN（テストを通す最小実装）

```
1. テストが通る最小限のコードを書く
2. 汚くてもOK、まず動かすことを優先
3. テストを実行して成功を確認
```

**ポイント**:
- 「正しい実装」ではなく「動く実装」を目指す
- 最小限のコードで済ませる

### Step 3: REFACTOR（コードを整理）

```
1. 重複を排除
2. 可読性を向上
3. パフォーマンスを最適化
4. テストが通ることを確認
```

**ポイント**:
- テストがあるから安心してリファクタできる
- リファクタ後もテストが通ればOK

---

## AIへの指示例

### パターン1: 単一機能のTDD

```
以下のテストが通るように実装してください。

テストファイル: tests/cart_test.py
[テストコードを貼る]

必要なファイルを作成し、テストが通ることを確認してください。
```

### パターン2: 複数アプローチの比較

```
以下のテストを3つの異なるアプローチで実装してください。

1. メモリ内実装
2. データベース実装  
3. 外部API連携実装

各実装で:
- テストが通ることを確認
- パフォーマンス特性をコメント
- トレードオフを記載
```

### パターン3: エッジケースの追加

```
既存のテストに以下のケースを追加してください。

追加するテストケース:
1. 在庫切れ商品の処理
2. 数量が0になった場合
3. 同じ商品を複数回追加

各ケースのテストと実装をしてください。
```

---

## テスト実行コマンド例

### Python (pytest)
```bash
# 全テスト実行
pytest

# 特定ファイルのみ
pytest tests/cart_test.py

# 特定のテスト関数のみ
pytest tests/cart_test.py::test_add_to_cart

# 失敗したテストのみ再実行
pytest --lf
```

### JavaScript (Jest)
```bash
# 全テスト実行
npm test

# 特定ファイルのみ
npm test cart.test.js

# ウォッチモード
npm test -- --watch

# カバレッジ測定
npm test -- --coverage
```

### Go
```bash
# 全テスト実行
go test ./...

# 特定パッケージのみ
go test ./cart

# カバレッジ測定
go test -cover ./...
```

---

## ベストプラクティス

### ✅ Do

- **テストは独立させる**: 他のテストに依存しない
- **1テスト1アサーション**: 可能な限り1つのことだけ検証
- **意味のある名前**: テスト名で何をテストしているか分かるように
- **Arrange-Act-Assert**: 準備→実行→検証の順序を守る

### ❌ Don't

- **sleep使わない**: タイミング依存のテストは避ける
- **本番データに依存しない**: テストデータは自己完結
- **実行順序に依存しない**: どの順序で実行しても通るように
- **ランダム値を使わない**: 再現性を保つ

---

## よくあるトラブルと対処法

### テストが遅い

```
対策:
- モックを活用して外部依存を削減
- データベースはトランザクションでロールバック
- 並列実行を検討
```

### テストが不安定（フレーキー）

```
対策:
- 非同期処理は適切に待つ
- タイムアウトを適切に設定
- テスト間の状態共有を排除
```

### 何をテストすべきか分からない

```
対策:
- Issueの受け入れ基準から始める
- 正常系→異常系→境界値の順で
- ビジネスロジックを優先
```

---

## まとめ

TDDは**最初は遅く感じるが、長期的には速い**開発手法です。

**メリット**:
- バグの早期発見
- 安全なリファクタリング
- 仕様書としてのテストコード
- 実装の品質保証

**AIと組み合わせると**:
- テストを書いてAIに実装させる
- 複数アプローチを並列実装
- テストが品質のゲートキーパーに

小さな機能から始めて、TDDの習慣を身につけましょう！
