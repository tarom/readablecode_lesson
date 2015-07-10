リーダブルコード勉強会


# 2章 名前に情報を詰め込む 例題


## 明確な単語を選ぶ

販売手数料を計算するメソッドのようだ。
getよりは適切な単語がありそうだ。

```
def get_reward_transaction_fee
  ...
end
```


## 汎用的な名前を避ける

企業の拠点(Buildingクラスの様子)のデータから、
CSVフォーマットの文字列を返すメソッドのようだ。

data, ret といった典型的なふわふわ変数名が使われている。
CSVを扱うなら、それぞれふさわしい名前があるはずだ。

```
def to_csv
  ret = []
  buildings.each do |building|
    data = []

    data << building.name
    data << building.address
    data << building.telephone
    ...

    ret << '"' + data1.join('","') + '"'
  end

  ret.join("\r\n") + "\r\n"
end
```


## 具体的な名前を使って、物事を詳細に説明する


target_params に"tags"というキーがあれば、"tag_list" に付け替える、
という単純な動作をするのだが、メソッド名から想像できるだろうか？
適切な名前をつけてやろう。

```
def update_all_params(target_params)
  target_params.xxx(...).tap do |params|
    params[:tag_list] ||= params.delete(:tags) if params.key?(:tags)
  end
end
```

## 変数名に大切な情報を付与する

```
```

## スコープの大きな変数には長い名前をつける

```
```

## 大文字やアンダースコアなどに意味を含める

```
```
