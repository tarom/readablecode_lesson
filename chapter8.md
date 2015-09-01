リーダブルコード勉強会

# 8章 巨大な式を分割する

## 説明/要約変数を使おう

同じ式が何度も登場していて辛いので、説明変数を使おう。

```haml
  - if current_customer && current_customer.available?(@package)
    - if current_customer.subscription_for(@package).expires_soon? && current_customer.subscription_for(@package).package_plan.name != 'monthly'
      -# もうすぐ期限が切れ
    - elsif current_customer && current_customer.purchased?(@package)
      -# 購入処理中である

  - if !(current_customer && current_customer.available?(@package))
    -# 価格＆購入ボタンを表示
```

## ド・モルガンの法則

rubyで良くあるパターン。
`if not`は`unless`に、`unless not`は`if`に書き換えたい。
その際にド・モルガンを使う。

```ruby
  100.times {
    a = rand(1..10)
    b = rand(1..10)
    break [a,b] unless !(a < b || a == b && a.odd?)
  }
```

## 短絡評価の悪用を避ける

&&と||の連鎖は複数繋げないほうがいいだろう。

```ruby
(f = File.open('hoge.txt', 'w')) && f.write('hoge') || print('file cannot open')
```

## 複雑なロジックと格闘する

複数の条件が絡み合っている場合は、小さい条件をガード節で早めに脱出させてゆく方法がある。

```ruby
man= OpenStruct.new(age: rand(18..50), income: rand(1..1000_0000), tall: rand(100..300))

# 20歳から35歳の間で年収700万なら結婚したいけど、身長が150以下は嫌。
if (20..35).cover? man.age && 700_0000 < man.income && !(tall < 150)
  print "marry me!"
else
  print "kimoi"
end
```

## 巨大な式を分割

読みづらすぎるので、要約変数を使って分解しよう。

```ruby
  # Customer(顧客)からSubscription(購読)を作って、
  # SubscriptionからPurchase(購入)を作って、PurchaseからBill(請求)をつくって、
  # Payment(支払い方法)に対してBillの決済処理を指示して、
  # tran(決済処理オブジェクト)を得る。
  tran = payment.pay!(Bill.create!(
    payment: payment,
    date: Time.zone.now.to_date,
    purchases: [
      customer.subscriptions.create!(
        package: package_price.package,
        package_plan: package_price.package_plan,
        ).purchases.create!(
        customer: customer,
        trainer: package_price.package.trainer,
        package_price: package_price,
        operation: :new,
        )
    ]))

  # tran を使う...
```
