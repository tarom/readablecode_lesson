リーダブルコード勉強会

# 3章 誤解されない名前 例題

## 範囲に含む？含まない？

このメソッドは、引数に日付の範囲をとるが、
終わりを含まないようだ。(日付に関しては良くある仕様だ。)
from / to よりもいい名前を本書が示しているはず。
```
def getHoge(from, to)
  Hoge.where(['? <= created_at AND created_at < ?, from, to])
end
```

## 真偽値変数・関数にはis/has/canを

以下の名前を、 is/has/can を使う形にrenameしてみよう
(rubyでは、メソッドに?をつけるのが一般的だからまた別問題。)

* autoload (true なら自動的にロードする)
* disableStubbing (true ならスタブ機能を切る)
* connetionPermitted (trueなら接続を許可する)

## 処理の重さを反映する

以下の名前を、なんか適切っぽくrenameしてみよう。

* getUserProfile (ネットワークアクセスする)
* updateEmail (データベースを書き換え、新しいアドレスにメールも送る)
