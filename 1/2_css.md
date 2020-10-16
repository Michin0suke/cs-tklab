# CSS

------

## CSSとは

CSSとは，Cascading Style Sheet (カスケーディング スタイル シート)の略称です。

CSSを使うことで，ホームページの見栄えを細かく指定することができます。HTMLが文書の「構造」を記述する言語であるのに対し，CSSは文書の「表現」を指定する言語ということになります。

例えば，CSSを使用すると文字に背景色を付けたり，枠を付けることができます。また，さらに細かい指定を行うことができる上に，Cascadingという言葉が示す通り，スタイルを多重に指定することも可能になります。

## CSSの書き方

CSSの書式はとてもシンプルです。

スタイルの指定は3つの文からなり，「どこの・何を・どうするか」の必要な情報のみを書き込むことで使用することができます。

それぞれ

* どこの

  = セクレタ(selector)

* 何を

  = プロパティ(property)

* どうするか

  = 値(value)

となります。



### タグに対するCSS指定

例えば，

body { background-color: red; }

と指定すると，セレクタが`body`タグ, プロパティが`background-color`で，値は`red`となります。これで文書本文全体の背景色が赤くなります。



### 特定idに対するCSS指定

特定のタグにのみCSSを指定したい時には，

\#red { color : red; }

と指定すると，セレクタ(`#`はタグ属性の`id`の指定を表わす)が`red`, プロパティが`color`で，値は`red`となります。これで`id="red"`と指定された特定タグの文字を赤くすることができます。



### 特定classに対するCSS指定

同様に

.red { color : red; }

と指定すると，セレクタ(`.`はタグ属性の`class`の指定を表わす)は`"red"`クラス指定されたタグ全部に対して指定されたことになります。



## CSSをHTMLに組み込む方法

CSSを組み込む方法は以下の3通りがあります。短いものはインライン，少しまとまってきたら内部参照にまとめ，他のHTMLファイルにも流用したい場合は外部参照を，という使い分けを行うのが普通です。

1. インライン

   HTMLタグの中に直接，スタイル属性(`style="プロパティ:値;..."`)としてを書き込むことCSSを記述する方法です。

   一番短くわかりやすい方法ですが，その分デザイン指定をすべてタグごとにソースコードに打ち込む必要があるので， 確認時わかりにくくなってしまいます。また，スタイルを属性として入力するので改行が使えず，書式が多少変わってきます。

2. 内部参照

   HTMLのHEAD要素内にスタイル要素(`<style>...<style>`)を設定し，CSSを記述する方法です。

   ソースコードとスタイル指定を分離できる上に，同じ見栄えにしたいタグはクラス属性(class)やid属性(id)へのスタイルとして一括で記述することができるので，全てをインラインで指定するより，スタイル要素内のまとまった箇所に短く書くことができるようになります。

   この方法での設定では，スタイル要素を書き込んだHTMLページに限定されたスタイルの指定しかできません。

3. 外部参照

   CSSを外部ファイルに作成し，そのファイルを参照することでスタイルを指定する方法です。

   外部参照を行うには次の2種類の方法があります。

   * link要素(`<link rel="stylesheet" href="スタイルシートのURL" />`)を使って参照する方法
   * `@import url(スタイルシートのURL)`構文を使って参照する方法

   この教材ではlink要素を使った外部参照を使います。

   

   外部参照のメリットは，異なるHTMLファイル間で同じスタイルを指定したい場合は同じスタイルシートを読み込めばいいので，複数のページで使いまわしができることです。

   当然，修正時はCSSファイルのみを直すことで，同じCSSファイルを使っているすべてのHTMLファイルのスタイルを修正することができるようになります。

### 簡単な例

例として，スタイルシートを使って様々な書式を変更してみましょう。以下，前述した3種類の方法でCSSを記述してみます。

1. インラインによる文字の大きさの変更

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <meta charset="utf-8">
       <title>HTMLサンプル</title>
   </head>
   <body>
       <p style="font-size: 10pt">文字サイズは10ポイントになります。</p>
       <p style="font-size: 12pt">文字サイズは12ポイントになります。</p>
       <p style="font-size: 14pt">文字サイズは14ポイントになります。</p>
       <p style="font-size: 16pt">文字サイズは16ポイントになります。</p>
       <p style="font-size: 18pt">文字サイズは18ポイントになります。</p>
   </body>
   </html>
   ```

   

   実行結果:

   [![img](http://cs-tklab.na-inet.jp/phpdb/Chapter1/fig/CSS2.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter1/fig/CSS2.PNG)

   

2. 内部参照による文字色の変更

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <meta charset="utf-8">
       <title>HTMLサンプル</title>
       <style>
           #aaa { color: red }
       </style>
   </head>
   <body>
       <p>ここの文字は黒です。</p>
       <p id="aaa">ここは文字が赤です。</p>
       <p>ここも文字は黒です。</p>
   </body>
   </html>
   ```

   

   実行結果:

   [![img](http://cs-tklab.na-inet.jp/phpdb/Chapter1/fig/CSS4.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter1/fig/CSS4.PNG)

   

3. 外部参照による背景と文字の色を変更

   下記のテキストファイルを`style.css`として作成します。

   ※スタイルファイルも文字コードはUTF-8を使用して保存すること！

   ```css
   /* 文書全体の背景色の指定 */
   body { background-color: #7FFFD4; }
   
   /* id="aaa"に対する指定 */
   #aaa { color: red; }
   
   /* class="bbb"に対する指定 */
   .bbb {
       font-weight: bold;
       color: blue;
   }
   ```

   

   この`style.css`を，以前作ったid_class_htmlファイルと同じフォルダに置き，下記のように6～7行目にこのスタイルファイルの読み込みを指定します。

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <meta charset="utf-8">
       <title>HTMLサンプル</title>
       <!-- スタイルシートの外部指定 -->
       <link href="style.css" rel="stylesheet">
   </head>
   <body>
       <p>ここの文字は黒です。</p>
       <p id="aaa">ここは文字が赤です。</p>
       <p>ここも文字は黒です。</p>
       <p class="bbb">ここは青くなります。</p>
       <address class="bbb">ここも青くなります。</address>
       <p><a href="#aaa">特定idへのリンク</a></p>
   </body>
   </html>
   ```

   

   実行結果:

   [![img](http://cs-tklab.na-inet.jp/phpdb/Chapter1/fig/css_id_class_html_browser.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter1/fig/css_id_class_html_browser.png)

   

以上，3つのスタイルファイルの使用例を見てきました。今後，CSSの指定を複数ファイルから構成されるWebアプリケーションに適用する時には，

1. Webアプリケーション全体の統一感を出すために，全てのHTMLに対して共通するCSS指定を外部参照用スタイルファイルとして用意して，全てのHTMLファイルでこのスタイルファイルを読み込むように設定し，
2. 特定のページにのみ適用させたいCSS指定は内部参照を使い，
3. 特定のタグにのみCSS指定したい時だけインライン指定する。

というように使い分けを行うようにしましょう。