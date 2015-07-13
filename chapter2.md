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

社内システムより。月間勤務時間を算出するメソッド。
このメソッド名に、本文と同じパターンで必要な情報を付与してみよう。

```
def total_work_time
  @attendances.to_a.sum(&:working_minute)
end

```

## スコープの大きな変数には長い名前をつける

javascriptのような言語では特に気をつけよう。
```
var f = false;
$(function(){
  var $btn = $('.show-dialog')

  $btn.click(function(){
    if(f) {
      // 既にダイアログを表示していたら、今表示しているデータを消していいか尋ねる...
      ...
    }else{
      // ダイアログを表示する
      ...
      f = true;
    }
  });
});
```

## 大文字やアンダースコアなどに意味を含める

jQueryを使ったコードだ。
jQueryオブジェクトには$をつけよう。
```
function loadScoreTable(){
  var table = $('#score-table');

  $.getJSON('http://...', {time: new Date()}, function(data){

    var sum = 0, count = 0;
    $.each(data.rows, function(i,v){
      var tr = $('<tr/>');
      tr.append($('<td/>').text(v.name));
      tr.append($('<td/>').text(v.score + '点'));
      table.append(tr);

      count ++;
      sum += v.score;
    });

    var tr_total = $('<tr/>');
    tr_total.append($('<td/>').text(count + '件'));
    tr_total.append($('<td/>').text('合計' + sum + '点');
    table.append(tr_total);
  });
}
```
