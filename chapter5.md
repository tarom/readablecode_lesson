リーダブルコード勉強会

# 5章 コメントすべきことを知る

## 不要なコメント

次のうち、不要なコメントがされている部分はどれだろう

```ruby
# 円周率 (精度はこれでも十分だろう)
MATH_PI = 3.1416

# 円の面積を求める
# radius: 半径
def calcAreaOfCircle(radius)
  # 円周率 * 半径 * 半径
  MATH_PI * radius * radius
end
```

## 記録すべき自分の考え

次のうち、不要なコメントはどれだろう

```ruby
# この処理はあんまり綺麗にかかれていない
hoge

# この処理はテーブルを集計するために必要
hage

# この処理は完璧ではないが、それなりな妥協点だ
huge
```

## 読み手の立場になって考える

このメソッドは読みやすいだろうか？
疑問点をあげて、適切なコメントを考えてみよう。

```
def get_reward_transaction_fee
  ...

  if nursery.bank_account.bank_code.start_with?("0036")
    return REWARD_TRANSACTION_FEE[:rakuten_bank][:normal]
  else
    if sum_payment >= 30000
      return REWARD_TRANSACTION_FEE[:other_bank][:great]
    else
      return REWARD_TRANSACTION_FEE[:other_bank][:less]
    end
  end
end
```
