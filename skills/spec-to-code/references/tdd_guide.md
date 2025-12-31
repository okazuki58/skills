# Test-Driven Development Workflow

このドキュメントは rails-ec-shop プロジェクトでのTDD開発フローを定義します。
AIによる並列実装においても、このフローに従ってください。

---

## TDDの基本サイクル

```
┌─────────────────────────────────────────┐
│ 1. RED: 失敗するテストを書く            │
│    - まず要件を明確化                   │
│    - 最小単位のテストから                │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│ 2. GREEN: テストを通す最小限の実装       │
│    - 汚くてもOK、まず動かす              │
│    - テストが通ることを最優先             │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│ 3. REFACTOR: コードを整理                │
│    - 重複排除                           │
│    - 可読性向上                         │
│    - パフォーマンス最適化                │
└─────────────────────────────────────────┘
              ↓
        (次の機能へ)
```

---

## 1. テストの粒度

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

    context 'when stock = 0' do
      let(:product) { create(:product, stock: 0) }
      it { expect(product.in_stock?).to be false }
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

  before do
    sign_in user
  end

  scenario 'user adds product to cart' do
    visit product_path(product)
    click_button 'Add to Cart'
    
    expect(page).to have_content('Added to cart')
    expect(page).to have_css('#cart-count', text: '1')
  end

  scenario 'user cannot add out-of-stock product' do
    out_of_stock = create(:product, :out_of_stock)
    visit product_path(out_of_stock)
    
    expect(page).not_to have_button('Add to Cart')
    expect(page).to have_content('Out of Stock')
  end
end
```

---

## 2. テストファイルの配置

```
spec/
├── models/
│   ├── product_spec.rb
│   ├── order_spec.rb
│   └── cart_item_spec.rb
├── services/
│   ├── create_order_service_spec.rb
│   └── apply_discount_service_spec.rb
├── features/
│   ├── shopping_cart_spec.rb
│   ├── checkout_spec.rb
│   └── user_registration_spec.rb
├── requests/
│   └── api/
│       └── products_spec.rb
└── support/
    ├── factory_bot.rb
    └── database_cleaner.rb
```

---

## 3. TDD実践例: カート機能

### Step 1: RED（失敗するテストを書く）

```ruby
# spec/features/shopping_cart_spec.rb
RSpec.describe "Shopping Cart", type: :feature do
  let(:user) { create(:user) }
  let(:product) { create(:product, name: "Ruby Book", price: 3000) }

  before { sign_in user }

  scenario 'user adds product to cart' do
    visit product_path(product)
    click_button 'Add to Cart'
    
    expect(page).to have_content('Added to cart')
    expect(current_path).to eq(cart_path)
    expect(page).to have_content('Ruby Book')
    expect(page).to have_content('¥3,000')
  end
end
```

**実行結果**: `FAILED` (まだカート機能を実装していないため)

### Step 2: GREEN（テストを通す最小実装）

```ruby
# app/controllers/cart_items_controller.rb
class CartItemsController < ApplicationController
  before_action :authenticate_user!

  def create
    product = Product.find(params[:product_id])
    cart = current_user.cart || current_user.create_cart
    
    cart.cart_items.create!(product: product, quantity: 1)
    
    redirect_to cart_path, notice: 'Added to cart'
  end
end
```

```ruby
# app/models/cart.rb
class Cart < ApplicationRecord
  belongs_to :user
  has_many :cart_items, dependent: :destroy
end
```

```ruby
# app/models/cart_item.rb
class CartItem < ApplicationRecord
  belongs_to :cart
  belongs_to :product
  
  validates :quantity, presence: true, numericality: { greater_than: 0 }
end
```

**実行結果**: `PASSED` ✅

### Step 3: REFACTOR（リファクタリング）

```ruby
# app/services/add_to_cart_service.rb
class AddToCartService
  def initialize(user)
    @user = user
  end

  def call(product, quantity = 1)
    cart = find_or_create_cart
    cart_item = find_or_initialize_cart_item(cart, product)
    
    cart_item.quantity += quantity
    cart_item.save!
    
    cart_item
  end

  private

  def find_or_create_cart
    @user.cart || @user.create_cart
  end

  def find_or_initialize_cart_item(cart, product)
    cart.cart_items.find_or_initialize_by(product: product) do |item|
      item.quantity = 0
    end
  end
end
```

```ruby
# app/controllers/cart_items_controller.rb
class CartItemsController < ApplicationController
  before_action :authenticate_user!

  def create
    product = Product.find(params[:product_id])
    AddToCartService.new(current_user).call(product)
    
    redirect_to cart_path, notice: 'Added to cart'
  rescue ActiveRecord::RecordInvalid => e
    redirect_to product_path(product), alert: e.message
  end
end
```

**実行結果**: `PASSED` ✅（リファクタ後もテスト通過）

---

## 4. AIへの指示方法

### パターンA: 単一機能の実装

```
以下のテストを読んで、テストが通るように実装してください。
@rails_standards.md の実装標準に従ってください。

テストファイル: spec/features/shopping_cart_spec.rb
[テストコードを貼る]

必要なファイル:
- app/controllers/cart_items_controller.rb
- app/models/cart.rb
- app/models/cart_item.rb
- app/services/add_to_cart_service.rb (リファクタ版)
- db/migrate/xxx_create_carts.rb
- db/migrate/xxx_create_cart_items.rb

すべてのファイルを作成し、テストが通ることを確認してください。
```

### パターンB: 複数パターンの並列実装

```
以下のテストを読んで、3つの異なるアプローチで実装してください。

テストファイル: spec/features/shopping_cart_spec.rb
[テストコードを貼る]

実装パターン:
1. Redis sessions版（Session[:cart]に保存）
2. Database版（Cartモデル + CartItemモデル）
3. Hybrid版（Redisで一時保存 → 購入時にDB保存）

各パターンで:
- テストが通ることを確認
- パフォーマンス特性をコメントで記載
- トレードオフを明記

@rails_standards.md に従って実装してください。
```

### パターンC: エッジケースの追加

```
既存のテストに、以下のエッジケースを追加してください。

現在のテスト: spec/features/shopping_cart_spec.rb
[既存テストコードを貼る]

追加するエッジケース:
1. 在庫切れ商品はカートに入れられない
2. カート内の商品が在庫切れになった場合の挙動
3. 同じ商品を複数回追加した場合、数量が加算される
4. カート内商品の数量を0にすると削除される

各ケースに対するテストを追加し、実装してください。
```

---

## 5. テストデータ管理（FactoryBot）

### 基本的なFactory定義

```ruby
# spec/factories/products.rb
FactoryBot.define do
  factory :product do
    sequence(:name) { |n| "Product #{n}" }
    description { "Test product description" }
    price { 1000 }
    stock { 10 }
    published { true }
    
    trait :out_of_stock do
      stock { 0 }
    end
    
    trait :unpublished do
      published { false }
    end
    
    trait :expensive do
      price { 10000 }
    end
  end
end
```

### 使用例

```ruby
# 基本的な使い方
product = create(:product)

# Traitを使った使い方
out_of_stock_product = create(:product, :out_of_stock)
expensive_product = create(:product, :expensive, price: 50000)

# 複数Trait
unpublished_out_of_stock = create(:product, :unpublished, :out_of_stock)

# 属性の上書き
custom_product = create(:product, name: "Custom Product", price: 5000)
```

---

## 6. テスト実行コマンド

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

# カバレッジ測定
COVERAGE=true bundle exec rspec
```

---

## 7. CI/CD統合

### GitHub Actions設定例

```yaml
# .github/workflows/test.yml
name: RSpec Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    
    services:
      postgres:
        image: postgres:14
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
      
      redis:
        image: redis:7
        options: >-
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: 3.2
          bundler-cache: true
      
      - name: Setup Database
        env:
          DATABASE_URL: postgres://postgres:postgres@localhost:5432/test
        run: |
          bundle exec rails db:create
          bundle exec rails db:schema:load
      
      - name: Run RSpec
        env:
          DATABASE_URL: postgres://postgres:postgres@localhost:5432/test
          REDIS_URL: redis://localhost:6379/0
        run: bundle exec rspec
      
      - name: Upload Coverage
        uses: actions/upload-artifact@v3
        with:
          name: coverage
          path: coverage/
```

---

## 8. テストのベストプラクティス

### ✅ Do
- テストは独立させる（他のテストに依存しない）
- 1テスト1アサーション（可能な限り）
- Arrange-Act-Assert パターンを使う
- 意味のあるテスト名をつける
- Factoryを活用してテストデータを管理

### ❌ Don't
- テストでsleep使わない（非同期処理は適切に待つ）
- 本番データに依存しない
- テストの順序に依存しない
- テストでランダム値を使わない（再現性を保つ）

---

## 9. AIによる並列TDD実行フロー

```
┌─────────────────────────────────────────┐
│ 1. ローカルでテストを書く                │
│    spec/features/shopping_cart_spec.rb  │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│ 2. クラウド環境で並列実装                │
│    - Agent 1: Redis版                   │
│    - Agent 2: DB版                      │
│    - Agent 3: Hybrid版                  │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│ 3. 各Agentがテスト実行                  │
│    - Agent 1: ✓ PASSED                  │
│    - Agent 2: ✗ FAILED                  │
│    - Agent 3: ✓ PASSED                  │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│ 4. 通過したブランチだけローカルに取得    │
│    git fetch origin agent-1             │
│    git fetch origin agent-3             │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│ 5. コードレビュー & 最終決定             │
│    - パフォーマンス                     │
│    - 可読性                             │
│    - メンテナンス性                      │
└─────────────────────────────────────────┘
```

---

## 10. トラブルシューティング

### テストが遅い場合
```ruby
# spec/rails_helper.rb
RSpec.configure do |config|
  # Database Cleanerを使う
  config.use_transactional_fixtures = false
  
  config.before(:suite) do
    DatabaseCleaner.clean_with(:truncation)
  end

  config.before(:each) do
    DatabaseCleaner.strategy = :transaction
  end

  config.before(:each, type: :feature) do
    DatabaseCleaner.strategy = :truncation
  end
end
```

### Capybaraのタイムアウト
```ruby
# spec/support/capybara.rb
Capybara.default_max_wait_time = 5 # デフォルトは2秒
```

---

## まとめ

TDDは**最初は遅く感じるが、長期的には速い**開発手法です。

**メリット**:
- バグの早期発見
- リファクタリングの安全性
- 仕様書としてのテストコード
- AIによる実装品質の担保

**AIと組み合わせると**:
- 複数アプローチを並列実装
- テストがゲートキーパーになる
- 手動レビューの負荷軽減

まずは小さな機能から始めて、TDDの習慣を身につけましょう！
