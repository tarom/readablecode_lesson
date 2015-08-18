リーダルブルコード勉強会

# 9章 変数と読みやすさ

## 問題1. 邪魔な変数を削除する

余計な変数がないか考えてみよう

```javascript
// 配列の中身をインデックスをつけて結合する
function indexed_join(array)  {
  result = ''
  for(var i = 0; i < array.length; i++) {
    index = i;
    separator = '_';
    string = array[i];
    indexed_string = index + seprator + string;
    result += indexed_string;
  }
  return result;
}
```

## 問題2. 変数のスコープをできるだけ小さくする

JavaScript の変数スコープを考慮に入れて、なるべくスコープの小さいコードにしよう

```javascript
function get_index(array, obj):
  i = 0;
  for(var value in array) {
    if(value == obj) {
      result = i;
      break;
    }
    i++;
  }
  return result;
}
```

## 問題3. 一度だけ書き込む変数を使う

問題かんがえるのむずかしい
