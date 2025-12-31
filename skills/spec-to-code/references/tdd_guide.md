# Test-Driven Development (TDD) Guide

テスト駆動開発の実践方法。言語・フレームワーク非依存。

---

## TDDサイクル

```
RED: 失敗するテストを書く
  ↓
GREEN: テストを通す最小実装
  ↓
REFACTOR: コードを整理
```

---

## 想定される使い方

### 1. テスト作成

```
Issue #1のテストを書いてください。
テストファイル: tests/feature_test.{拡張子}
```

### 2. 実装

```
{テストファイル名} のテストが通るように実装してください。
```

---

## テストの書き方

### 単体テスト

```python
# Python
def test_add_to_cart():
    cart = Cart()
    cart.add(Product(id=1, price=1000))
    
    assert cart.item_count() == 1
    assert cart.total() == 1000
```

```javascript
// JavaScript
test('adds product to cart', () => {
  const cart = new Cart();
  cart.add({ id: 1, price: 1000 });
  
  expect(cart.itemCount()).toBe(1);
  expect(cart.total()).toBe(1000);
});
```

### 統合テスト

```python
# Python + Selenium
def test_user_adds_to_cart():
    browser.visit('/products/1')
    browser.click('Add to Cart')
    
    assert browser.find('#cart-count').text == '1'
```

```javascript
// JavaScript + Playwright
test('user adds to cart', async ({ page }) => {
  await page.goto('/products/1');
  await page.click('text=Add to Cart');
  
  await expect(page.locator('#cart-count')).toHaveText('1');
});
```

---

## TDD実装フロー

### RED: 失敗するテストを書く
1. Issueの受け入れ基準を読む
2. 最小単位のテストから書く
3. 実行して失敗を確認

### GREEN: テストを通す最小実装
1. テストが通る最小限のコードを書く
2. 汚くてもOK、まず動かす
3. 実行して成功を確認

### REFACTOR: コードを整理
1. 重複を排除
2. 可読性を向上
3. テストが通ることを確認

---

## AIへの指示例

### 単一機能のTDD
```
以下のテストが通るように実装してください。

[テストコードを貼る]

必要なファイルを作成してください。
```

### 複数アプローチの比較
```
以下のテストを3つの方法で実装してください。

1. メモリ内実装
2. データベース実装
3. 外部API実装

各実装でテストを通し、トレードオフをコメントしてください。
```

### エッジケースの追加
```
既存のテストに以下を追加してください。

1. 在庫切れ商品の処理
2. 数量0の処理
3. 重複追加の処理
```

---

## テスト実行コマンド

### Python (pytest)
```bash
pytest                          # 全テスト
pytest tests/cart_test.py       # 特定ファイル
pytest --lf                     # 失敗したテストのみ
```

### JavaScript (Jest)
```bash
npm test                        # 全テスト
npm test cart.test.js          # 特定ファイル
npm test -- --watch            # ウォッチモード
```

### Go
```bash
go test ./...                  # 全テスト
go test ./cart                 # 特定パッケージ
go test -cover ./...           # カバレッジ
```

---

## ベストプラクティス

**Do:**
- テストは独立させる
- 1テスト1アサーション
- 意味のある名前
- Arrange-Act-Assert順序

**Don't:**
- sleep使わない
- 本番データに依存しない
- 実行順序に依存しない
- ランダム値使わない

---

## トラブル対処

**テストが遅い**: モック活用、トランザクションロールバック、並列実行

**フレーキー**: 非同期処理を適切に待つ、テスト間の状態共有を排除

**何をテストすべきか不明**: 受け入れ基準から始める、正常系→異常系→境界値の順
