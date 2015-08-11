リーダルブルコード勉強会

# 4章 美しさ

## 例題1

改行位置とスペースを駆使して良い感じにしましょう

- コードの「列」を整列すれば、概要が把握しやすくなる。
- 複数のコードブロックで同じようなことをしていたら、シルエットも同じようなものにする。


```ruby
let(:nursery_class_tulip) {
  create(
    :nursery_class,
    nursery: nursery,
    name: 'ちゅーりっぷ'
  )
}
let(:nursery_class_dandelion) {
  create(:nursery_class,
    nursery: nursery,
    name: 'たんぽぽ'
  )
}
let(:nursery_class_viola) {
  create(:nursery_class,
    nursery: nursery,
    name: 'すみれ'
  )
}
```

## 例題2

フォームの定義を参考に Ruby のコードを良い感じにしましょう

- ある場所で A・B・C のように並んでいたものを、他の場所で B・C・A の ように並べてはいけない。意味のある順番を選んで、常にその順番を守る。

### フォームの定義

```haml
%form(action="/post")
  %input(type="text" name="name")
  %input(type="text" name="address")
  %input(type="text" name="telephone")
  %input(type="text" name="email")
```

### 直したいコード

```ruby
User.create!(
  name: params[:name],
  email: params[:email],
  telephone: params[:telephone],
  address: params[:address],
)

```

## 例題3

コメントなどをつかってRubyのコードを良い感じにしましょう

- 空行を使って大きなブロックを論理的な「段落」に分ける。

```ruby
nursery.content_orders << create(:order, nursery: nursery, browsing_method: :printing, number: 2)
nursery.content_orders << create(:order, nursery: nursery, browsing_method: :printing, number: 3)
nursery.content_orders << create(:order, nursery: nursery, browsing_method: :printing, number: 1)
nursery.content_orders << create(:order, nursery: nursery, browsing_method: :printing, number: 2)
nursery.content_orders << create(:order, nursery: nursery, browsing_method: :printing, number: 1)
nursery.content_orders << create(:order, nursery: nursery, browsing_method: :download, number: 2)
nursery.content_orders << create(:order, nursery: nursery, browsing_method: :download, number: 1)
nursery.content_orders << create(:order, nursery: nursery, browsing_method: :download, number: 2)
nursery.content_orders << create(:order, nursery: nursery, browsing_method: :download, number: 1)
nursery.content_orders << create(:order, nursery: nursery, browsing_method: :download, number: 2)
another_nursery.content_orders << create(:order, nursery: another_nursery, browsing_method: :printing, number: 1)
another_nursery.content_orders << create(:order, nursery: another_nursery, browsing_method: :printing, number: 2)
another_nursery.content_orders << create(:order, nursery: another_nursery, browsing_method: :download, number: 1)
another_nursery.content_orders << create(:order, nursery: another_nursery, browsing_method: :download, number: 1)
```
