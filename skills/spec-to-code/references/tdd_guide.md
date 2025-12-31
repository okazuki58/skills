# Test-Driven Development Workflow

このドキュメントはTDD開発フローを定義します。言語・フレームワーク非依存です。

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

## テストの粒度

### 単体テスト（Model/Service）
```ruby
# spec/models/product_spec.rb
RSpec.describe Product, type: :model do
  describe 'validations' do
    it { should validate_presence_of(:name) }
    it { should validate_numericality_of(:price).is_greater_than(0) }
  end

  describe '#in_stock?' do
    context 'when stock > 0' do
      let(:product) { create(:product, stock: 10) }
      it { expect(product.in_stock?).to be true }
    end
  end
end
```

### 統合テスト（Feature/System）
```ruby
# spec/features/shopping_cart_spec.rb
RSpec.describe "Shopping Cart", type: :feature do
  let(:user) { create(:user) }
  let(:product) { create(:product, name: "Test Product", price: 1000) }

  before { sign_in user }

  scenario 'user adds product to cart' do
    visit product_path(product)
    click_button 'Add to Cart'
    
    expect(page).to have_content('Added to cart')
    expect(page).to have_css('#cart-count', text: '1')
  end
end
```

---

## TDD実践例

### Step 1: RED（失敗するテストを書く）

要件を明確化し、最小単位のテストから書き始めます。

### Step 2: GREEN（テストを通す最小実装）

汚くてもOK。まず動かすことを最優先します。

### Step 3: REFACTOR（リファクタリング）

- 重複排除
- 可読性向上
- パフォーマンス最適化

---

## AIへの指示方法

### パターンA: 単一機能の実装

```
以下のテストを読んで、テストが通るように実装してください。

テストファイル: spec/features/shopping_cart_spec.rb
[テストコードを貼る]

必要なファイルを全て作成し、テストが通ることを確認してください。
```

### パターンB: 複数パターンの並列実装

```
以下のテストを読んで、3つの異なるアプローチで実装してください。

実装パターン:
1. Redis sessions版
2. Database版
3. Hybrid版

各パターンで:
- テストが通ることを確認
- パフォーマンス特性をコメント
- トレードオフを明記
```

---

## テスト実行コマンド

```bash
# 全テスト実行
bundle exec rspec

# 特定のファイルだけ実行
bundle exec rspec spec/models/product_spec.rb

# 特定の行番号のテストだけ実行
bundle exec rspec spec/features/shopping_cart_spec.rb:25

# Feature testだけ実行
bundle exec rspec spec/features

# 失敗したテストだけ再実行
bundle exec rspec --only-failures
```

---

## ベストプラクティス

### ✅ Do
- テストは独立させる（他のテストに依存しない）
- 1テスト1アサーション（可能な限り）
- Arrange-Act-Assert パターンを使う
- 意味のあるテスト名をつける

### ❌ Don't
- テストでsleep使わない
- 本番データに依存しない
- テストの順序に依存しない
- テストでランダム値を使わない

---

## まとめ

TDDは最初は遅く感じますが、長期的には速い開発手法です。

**メリット**:
- バグの早期発見
- リファクタリングの安全性
- 仕様書としてのテストコード
- AIによる実装品質の担保
