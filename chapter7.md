リーダルブルコード勉強会

# 4章 美しさ

## 問題1. if 文の順番

```ruby
def welcome_message
  if not user.admin?
    "ドーモ、#{user.name}＝サン。あなたは管理者です。"
  else
    "こんにちは、#{user.name}さん。"
  end
end
```

## 問題2. ネストを浅くする1

```ruby
# 支払い情報を登録しているユーザはキャンペーンに登録できない
def must_not_has_payment
  if not campaign.discount_name.invitation_code?
    if (not campaign.discount_name.pricecut?) and (not campaign.discount_name.trial_period?)
      if user and user.payment.present?
        errors.add(:base, VALIDATE_MESSAGE) unless errors.added?(:base, VALIDATE_MESSAGE)
      end
    end
  end
end
```

## 問題3. いろいろ混ぜてみた

```ruby
def import_csv(csv)
  csv_array = CSV.load(csv)
  first_row = csv_array.shift
  if first_row[0] != "ID" and "Name" != first_row[1] and first_row[2] != "Email"
    csv_array.each do |row|
      if row[0].empty?
        next
      elsif not row[0] =~ /\d+/
        raise "不正なCSVデータです。"
      else
        User.create(
          id: row[0],
          name: row[1],
          email: row[2],
          address: row[3],
        )
      end
    end
  else
    raise "不正なCSVデータです"
  end
end
```
