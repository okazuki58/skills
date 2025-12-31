# GitHub Issue Template

このテンプレートは、SPEC.mdから自動的にGitHub Issueを生成するためのフォーマットです。

---

## Issue基本情報

**Title**: [機能名] - [簡潔な説明]

例: `カート機能 - 商品をカートに追加できるようにする`

---

## 📋 概要

[この機能の目的を1-2文で説明]

---

## 🎯 Acceptance Criteria（受け入れ条件）

機能が完成したと判断できる条件をチェックリスト形式で記載：

- [ ] ユーザーは商品詳細ページから「カートに追加」ボタンをクリックできる
- [ ] カートに追加すると、カート画面にリダイレクトされる
- [ ] カート画面に追加した商品が表示される
- [ ] 在庫切れ商品は「カートに追加」ボタンが非表示になる
- [ ] 同じ商品を複数回追加すると、数量が加算される

---

## 📝 仕様詳細

### データモデル
```
Cart
  - id: integer
  - user_id: integer
  - created_at: datetime
  - updated_at: datetime

CartItem
  - id: integer
  - cart_id: integer
  - product_id: integer
  - quantity: integer
  - created_at: datetime
  - updated_at: datetime
```

### UI/UX
- 商品詳細ページに「カートに追加」ボタンを配置
- ボタンクリック後、Turbo Streamでカート数を更新
- カート画面では、商品一覧と合計金額を表示

### エッジケース
- 在庫切れ商品: ボタン非表示 + 「在庫切れ」表示
- 未ログインユーザー: ログイン画面へリダイレクト
- 数量0: カートから商品を削除

---

## 🧪 Test Scenarios

### Feature Tests
```ruby
# spec/features/shopping_cart_spec.rb

RSpec.describe "Shopping Cart", type: :feature do
  let(:user) { create(:user) }
  let(:product) { create(:product, name: "Test Product", price: 1000, stock: 10) }

  before { sign_in user }

  scenario 'user adds product to cart' do
    visit product_path(product)
    click_button 'Add to Cart'
    
    expect(page).to have_content('Added to cart')
    expect(current_path).to eq(cart_path)
    expect(page).to have_content('Test Product')
    expect(page).to have_content('¥1,000')
  end

  scenario 'user cannot add out-of-stock product' do
    out_of_stock = create(:product, :out_of_stock)
    visit product_path(out_of_stock)
    
    expect(page).not_to have_button('Add to Cart')
    expect(page).to have_content('Out of Stock')
  end

  scenario 'adding same product increases quantity' do
    visit product_path(product)
    click_button 'Add to Cart'
    visit product_path(product)
    click_button 'Add to Cart'
    
    visit cart_path
    expect(page).to have_css('.quantity', text: '2')
  end
end
```

### Model Tests
```ruby
# spec/models/cart_item_spec.rb

RSpec.describe CartItem, type: :model do
  describe 'validations' do
    it { should validate_presence_of(:quantity) }
    it { should validate_numericality_of(:quantity).is_greater_than(0) }
  end

  describe 'associations' do
    it { should belong_to(:cart) }
    it { should belong_to(:product) }
  end
end
```

---

## 📂 実装ファイル

実装が必要なファイル一覧：

### Models
- [ ] `app/models/cart.rb`
- [ ] `app/models/cart_item.rb`

### Controllers
- [ ] `app/controllers/carts_controller.rb`
- [ ] `app/controllers/cart_items_controller.rb`

### Views
- [ ] `app/views/carts/show.html.erb`
- [ ] `app/views/products/_add_to_cart_button.html.erb`

### Services（必要に応じて）
- [ ] `app/services/add_to_cart_service.rb`

### Migrations
- [ ] `db/migrate/xxx_create_carts.rb`
- [ ] `db/migrate/xxx_create_cart_items.rb`

### Tests
- [ ] `spec/models/cart_spec.rb`
- [ ] `spec/models/cart_item_spec.rb`
- [ ] `spec/features/shopping_cart_spec.rb`
- [ ] `spec/services/add_to_cart_service_spec.rb`

### Factories
- [ ] `spec/factories/carts.rb`
- [ ] `spec/factories/cart_items.rb`

---

## 🔧 技術的な実装方針

### アプローチ
- Hotwire（Turbo Streams）で部分更新
- Service Objectでビジネスロジックを分離
- Redis sessionsでカート情報を管理（パフォーマンス重視）

### 代替案
- DB sessionsで管理（データ永続性重視）
- Hybrid: Redisで一時保存 → 購入時にDB保存

### トレードオフ
- **Redis**: 速いが、Redisダウン時にデータ消失
- **DB**: 永続化できるが、負荷が高い

今回は**Redis**を採用（理由: ECサイトでは速度が重要）

---

## 📚 参考資料

- [Rails Guide: Active Record Associations](https://guides.rubyonrails.org/association_basics.html)
- [Hotwire Documentation](https://hotwired.dev/)
- [RSpec Best Practices](https://rspec.info/)
- 類似実装: [他のECサイトの参考コード]

---

## 🏷️ Labels

- `feature`: 新機能
- `priority: high`: 優先度高
- `tdd`: テスト駆動開発
- `good first issue`: 初心者向け（該当する場合）

---

## 👥 Assignees

- @okazuki58

---

## 🗓️ Milestone

- Sprint 1: 基本機能（カート追加・表示）
- Sprint 2: エッジケース対応（在庫管理連携）

---

## 📌 Related Issues

- #XX: 在庫管理機能（依存）
- #YY: 注文機能（関連）

---

## 💬 Discussion Points

実装前に議論したい点があればここに記載：

- [ ] Redis vs DB、どちらを優先すべきか？
- [ ] カートの有効期限は？（30日？90日？）
- [ ] ゲストユーザーのカート機能は必要か？

---

## ✅ Definition of Done

この機能が「完了」と判断できる条件：

- [ ] 全てのAcceptance Criteriaを満たしている
- [ ] テストが全て通る（RSpec: GREEN）
- [ ] コードレビューが完了している
- [ ] ドキュメント（README）が更新されている
- [ ] mainブランチにマージされている

---

## 🚀 実装手順（推奨）

1. **テストを先に書く（RED）**
   - `spec/features/shopping_cart_spec.rb`
   - `spec/models/cart_spec.rb`
   - `spec/models/cart_item_spec.rb`

2. **最小実装（GREEN）**
   - Models作成
   - Controllers作成
   - Views作成

3. **リファクタリング（REFACTOR）**
   - Service Objectに抽出
   - N+1対策
   - コードの整理

4. **レビュー & マージ**
   - Pull Request作成
   - コードレビュー
   - CI/CDでテスト通過確認
   - mainへマージ

---

## 📝 Notes

その他、実装中に気づいたことや改善点をここに記録：

- 
