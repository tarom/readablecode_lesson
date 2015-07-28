リーダブルコード勉強会

# 6章 コメントは正確で簡潔に

## 複数のものを指す可能性がある代名詞を避ける

あいまいな代名詞を置き換えよう
```ruby
def hogehoge(a, b)
  # この引数ゼロだとエラーになるので、ゼロを返す
  a / b rescue 0
end
```

## 関数の動作はできるだけ正確に説明する

あいまいな動作をより正確にしてみよう

```ruby
# 1から100までの数字を表示する。
# ただし、3の倍数のときはfizzを、5の倍数のときはbuzを代わりに表示する
def fizzbuz
  1.upto(100).each do |i|
    p 'fizz' if i % 3 == 0
    p 'buz' if i % 5 == 0
    p i if i % 3 != 0 && i % 5 != 0
  end
end
```

## コメントに含める入出力の例を慎重に選ぶ

適切な例を考えてみよう

```ruby
# 正しいHTML開始タグかチェックする
# 三角括弧で囲まれていて、最初にタグ名があり、残りは属性の列挙
# 属性は="..."で値を設定できるが、単体でも存在可能である
# 例:
#  .... => OK
#  .... => NG
def validate_html_tag_start(value)
  unless value =~ /.../
    raise "Invalid HTML Tag"
  end
end
```

## コードの意図は、詳細レベルではなく、高レベルで記述する

以下のコメントは詳細すぎる。意図が分かるだろうか？
意図で書いてみよう。

```ruby
# published_atが期間内の記事一覧から重複がないようにauthor_idを数える
Article.where(published_at: term).map(&:author_id).uniq.count
```

## よくわからない引数にはインラインコメントを使う

キーワード引数のない言語は辛いという例。
hogehogeの呼び出しに対して、本を参考に適切にコメントを入れよう。


```ruby
def hogehoge(force_update, dry_run)
  ...
end

# この呼びだし
hogehoge(true, false)
```

## 多くの意味が詰め込まれた言葉や表現を使って、コメントを簡潔に保つ

なにか長い説明があるが、適切で端的な言葉があるはず

```ruby
# 新しい空っぽのマップを返す
def createEmptyMap
  # 配列の中に配列を入れてその中に更にHashを入れる
  map = []
  12.times do
    temp = []
    6.times { temp << { ... } }
    map << temp
  end
  map
end
```
