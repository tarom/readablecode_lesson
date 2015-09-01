リーダブルコード勉強会

# 10章 無関係の下位問題を抽出する

## ユーティリティコードを分離する

javascriptで、検索フォームをajax化するコードを組んでみた。
この中からユーティリティコードを分離してみよう。

```javascript
  // form#searchの内容を元に、ユーザーを検索する
  function search_users() {
    var params = '';

    // 検索フォームから値を読んで、パラメーターを組み立てる
    $('form#search).find('input, textarea, select').each(function(){
      if(params.length == 0) {
        params += '?';
      }else
        params += '&';
      }
      var $this = $(this);
      params += $this.attr('name') + '=' + $this.val();
    });

    // paramsでusersを検索して、その結果をJSONで取ってくる
    $.getJSON('http://hogehoge.hoge/api/users?' + params, function(result){
      // JSONを使って一覧を更新...
      var $user_list = $('li#users');
    });
  }
```

## プロジェクト特有の機能も分割する

javascriptでスクレイピングをするコードの一部を組んでみた。
主にproductとcompanyを切り出しているようだが、このコードの下位問題とはなんだろうか。

```
  // 与えられたhtml文字列から製品情報と製造会社情報を探しだしてオブジェクトにして返す
  function scrape_product(html_str) {
    var $doc = $(html_str);

    var product = {};

    // 製品コードっぽいものを手がかりに当たりをつける
    var $title = $doc.find(':regex(text, PROD-.*)');
    // 最も近い<section>がきっと親である
    var $prod_root = $title.closest('section');

    //製品名
    product.name = $prod_root.find('h1,h2,h3,h4,h5,h6').eq(0).html();
    //価格
    product.price = $prod_root.find(':regex(text, [0-9]+円').eq(0).html();
    //販売時期
    product.publication = $prod_root.find(':regex(text, [0-9]{4}年').eq(0).html();
    ...

    var company = {};

    // 株式会社という文字列を頼りに探す
    var $title = $doc.find(':regex(text, "(株)|株式会社")');
    // 最も近い<section>がきっと親である
    var $com_root = $title.closest('section');

    //製品名
    company.name = $com_root.find('h1,h2,h3,h4,h5,h6').eq(0).html();
    //所在地 郵便番号を見つける。
    company.location = $com_root.find(':regex(text, [0-9]{3}-[0-9]{4}').eq(0).html();
    //設立年度
    company.established = $com_root.find(':regex(text, [0-9]{4}年.*設立').eq(0).html();
    ...

    return { 'product': product, 'company': company };
  }

```

## インターフェースを整える

統一されてないインターフェイスは辛い。

```ruby

# Otherライブラリ中の、ソースコードを触ってはいけないUserクラス。
# Rubyの書き方にそってなくてつらい
class Other::User
  def setName(name)
    @name = name
  end

  def getName
    @name
  end

  # 何故かバラバラに設定しかできない
  def setBirthYear(year)
    ...
  end

  def setBirthMonth(month)
    ...
  end

  def setBirthDay(day)
    ...
  end
end

namy = '名前'
birthday = Date.new(1900,1,1)

# こうなる。つらい。
other_user.setName(name)
other_user.birthyear = birthday.year
other_user.birthmonth = birthday.month
other_user.birthday = birthday.day


# 自分でwrapperを定義していい感じにしよう！
class User
  def initialize(user)
    @user = user
  end

  # ここにwrapperメソッドを定義
  ..
end

# いい感じに使おう！
user = User.new(other_user)

...
```
