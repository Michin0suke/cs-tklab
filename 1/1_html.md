# HTML

------

## HTMLとは

HTMLとは，HyperText Markup Language(ハイパーテキスト マークアップ ランゲージ)の頭文字をとったものです。

HTMLはWebページを作成するためのマークアップ言語です。タグ(tag)を構成的に積み上げていくことで，文書の構造を規定し，静的なWebページを作り上げていきます。

## タグ

タグは半角大小記号`<・・・>`で囲んで記述します。例えばHTMLタグの場合は

<html>
・・・
HTML要素
・・・
</html>

のように書きます。



タグは基本的に開始タグと終了タグに分かれています。開始のタグは`<タグ名>`のように書き，終了タグは`</タグ名>`のように`/`をつけて開始タグと区別します。開始タグと終了タグでひとつのセットになります。普通のブラウザではタグ名は大文字と小文字の区別をしませんので，どちらで書いても構いませんが，最近は小文字を使うケースが多いようです。

水平線(<hr>)や，改行(`<br>`)のように単独で使用するタグもあります。この場合は`<hr />`のようにタグ名の後に` /`を入れて，単独使用タグであることを区別することもあります。

## 要素

開始タグと終了タグに囲まれた範囲のことを要素(element)といいます。これはHTMLを構成する最も基本的な単位です。



HTMLを書いていくうえで必ず使用しなければならない要素がHTML要素であり，HTML要素の中に他の全ての要素を書き込んでいくことになります。

## 属性

タグには `color="red"`，`size="5"`，`class="small"`，`id="aaa"` などの 属性(attribute) を指定することができます。

`color`，`size`，`class`，`id`の部分を属性名(attribute name)，`red`，`5`，`small`，`aaa`の部分を属性値(attribute value)と呼びます。属性値は2重引用符（ダブルクォーテーション, double quotation)で囲むのが基本ですが，スペースの入らない文字列であれば省略しても通常は問題ありません。

下記の例は，FONT要素(「～」の部分）の文字サイズを5(通常は1)に拡大して表示しています。

```html
<font color=red size=5>〜</font>
```

## 基本的なタグ

基本的な４つのタグを説明します。このタグは必ずHTMLファイルに不可欠なものですので，しっかり覚えましょう。

### HTML要素

下記は「「～～」はHTML文です」と宣言しているタグです。

``` html
<html>～～～</html>
```



### ヘッダ

下記は，HTMLの情報を書くためのヘッダ(header)を宣言します。文字コードの指定，ページタイトル，著者，CSS等の情報をHEADER用として指定します。

```html
<head>～～～</head>
```



### Webページタイトル

ヘッダ要素の中に，ページのタイトルを宣言します。ブラウザウィンドウのヘッダ部分の表示や，ブックマーク時の表示名にこのTITLE要素が使用されます。

```html
<title>～～～</title>
```





### Webページ本文

Webページとしてブラウザに表示させたい内容を，このBODY要素に指定します。

```html
<body>～～～</body>
```



## DOCTYPE宣言

HTMLはW3Cという国際機関で標準仕様が定められており，いくつかのバージョンが存在します。バージョンによっては使用できない要素や属性値が存在します。

DOCTYPE宣言はこのHTMLのバージョンを宣言するために使用します。このDOCTYPE宣言を使用しなくてもHTMLは問題なく書くことができます。しかし，宣言しないと，HTMLのバージョンがブラウザが定めたものに規定されます。使用ユーザーのバージョンの違いにより要素の解釈が違ってしまうことがあるので，様々なブラウザから閲覧される可能性のあるWebページに対しては，HTMLのバージョン違いによって生じる差異をなくすために，このDOCTYPE宣言は必須のものと言えるでしょう。

現在使用されている主なHTMLのバージョンと，そのDOCTYPE宣言をまとめたものが下記の表になります。

| HTML規格                                   | 型                                                           | DOCTYPE宣言                                                  |
| :----------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| [HTML4.01](https://www.w3.org/TR/html401/) | strict                                                       | ‹!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd"› |
| transitional                               | ‹!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd"› |                                                              |
| [XHTML1.0](https://www.w3.org/TR/xhtml1/)  | strict                                                       | ‹!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd"› |
| transitional                               | ‹!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"› |                                                              |
| [HTML5](https://www.w3.org/TR/html5/)      |                                                              | ‹!DOCTYPE html›                                              |



本教材では以降，DOCTYPE宣言はHTML5を基準とし，使用する文字コードはUTF-8 (8bits単位のUnicodeエンコード方式)に統一します。従って，下記のようなHTMLファイルを`"template.html"`という名前で作成して保存しておき，これをコピペして使用していきます。

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>ここにタイトルを入れます。</title>
</head>
<body>
    ここに内容を入れます。
</body>
</html>
```





**[注意！]** `template.html`を保存する時には必ず文字コードをUTF-8に指定して保存して下さい。保存したら必ずブラウザで開き，文字化けしていないかどうか確認して下さい。

## 簡単なタグの使用例

ここで簡単なタグの使用例をお見せしますが，これ以外にも多くのタグが存在しますので，必要に応じてタグ辞典のようなものを参照して使用して下さい。

※HTMLファイル名については特に指定しませんが，日本語は避け，いわゆる半角英数字(ASCIIコードの範囲内の文字)で指定して下さい。また，HTMLファイルの拡張子は必ず`.html`にして下さい。

### 段落

Pタグの要素が一段落になります。特に指定しない限り，段落の間には1行分の空きができます。

#### HTMLファイル

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>HTMLサンプル</title>
</head>
<body>
    <p>ここは一つの段落になります。</p>
    p要素の前後は自動的に改行され、空白の行が表示されます。
</body>
</html>
```



#### ブラウザ表示例

![img](http://cs-tklab.na-inet.jp/phpdb/Chapter1/fig/html17.PNG)

### 改行

HTMLファイルでは改行コードは無視されます。従って，強制改行を行うにはBRタグを使用しなければなりません。

#### HTMLファイル

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>HTMLサンプル</title>
</head>
<body>
    あいうえお<br>かきくけこさしすせそ<br>太刀つてとなにぬねのはひふへほ
</body>
</html>
```

#### ブラウザ表示例

![img](http://cs-tklab.na-inet.jp/phpdb/Chapter1/fig/html15.PNG)

### 文字を装飾する

Bタグ(**太字**)，Iタグ(*斜体, イタリック*)，Uタグ(下線)の例です。斜体を使うと日本語では読みづらくなるので，なるべく避けましょう。

#### HTMLファイル

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>HTMLサンプル</title>
</head>
<body>
    <p><b>文字が太字になります。</b></p>
    <p><i>文字が斜体になります。</i></p>
    <p><u>文字に下線がつきます。</u></p>
</body>
</html>
```

#### ブラウザ表示例

![img](http://cs-tklab.na-inet.jp/phpdb/Chapter1/fig/html9.PNG)

### 一部の文字を変更する

文字サイズ，文字色をFONTタグの属性値として指定した例です。

#### HTMLファイル

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>HTMLサンプル</title>
</head>
<body>
    <p><font color=red>ここの部分は</font>文字が赤くなります。</p>
    <p><font size=7>ここの部分は</font>文字が大きくなります。</p>
</body>
</html>
```

#### ブラウザ表示例

![img](http://cs-tklab.na-inet.jp/phpdb/Chapter1/fig/html11.PNG)

### 見出し

見出しはh1～h6まであります。

#### HTMLファイル

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>HTMLサンプル</title>
</head>
<body>
    <h1>これが一番大きい見出しです。</h1>
    <h6>これが一番小さい見出しです。</h6>
</body>
</html>
```

#### ブラウザ表示例

![img](http://cs-tklab.na-inet.jp/phpdb/Chapter1/fig/html13.PNG)

### 表（テーブル）の作成

枠付きの表を作る時には`border`属性を使います。枠線の太さを表わす属性値は半角数字で指定します。CSS(Cascading Style Sheet)を利用してborder属性を変更すれば，もっとおしゃれな枠を付けることができます。

#### HTMLファイル

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>HTMLサンプル</title>
</head>
<body>
    <table border="1">
        <tr>
            <td>１つ目のセルになります。</td>
            <th>２つめのセルになります。</th>
        </tr>
        <tr>
            <td>１つ目のセルの下になります。</td>
            <th>その隣になります。</th>
        </tr>
    </table>
</body>
</html>
```

#### ブラウザ表示例

![img](http://cs-tklab.na-inet.jp/phpdb/Chapter1/fig/html19.PNG)

## タグのidとclass

HTML5では，HTMLは文書の構造を規定し，背景色やフォントなどの見かけのデザインはCSSで規定するというのが基本です。そこで活躍するのが，前述した`id`と`class`という属性です。

`id`属性は，IDentify(身元を確認する)という意味からも分かる通り，特定のタグに付けられる符号のようなものです。従って，同一HTMLファイル内では**`id="id名"`の属性値`id名`は唯一名称になるように**設定しなくてはなりません。

これに対して，`class`属性は，タグのグループを指定するものなので，**同一のクラスに属する複数のタグに対して同じ属性値を使用します。**

下記のサンプルでは，`aaa`というidを持つタグと，`bbb`というclass名を持つタグが混在しています。

idは固有のものなので，同一HTMLファイル内におけるリンク先としても使用できます。この場合は，シャープマーク`#`をid属性値に付加して指定します。

#### HTMLファイル: id_class.html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>HTMLサンプル</title>
</head>
<body>
    <h1>idとclass</h1>
    <p>ここの文字は黒です。</p>
    <p id="aaa">ここは文字が赤になります。</p>
    <p>ここも文字は黒です。</p>
    <p class="bbb">ここは青くなります。</p>
    <address class="bbb">ここも青くなります。</address>
    <p><a href="#aaa">特定idへのリンク</a></p>
</body>
</html>
```

上記のファイルを表示しても特に表記が変わるわけではなく，下記のように表示されるだけです。

#### ブラウザ表示例



![img](http://cs-tklab.na-inet.jp/phpdb/Chapter1/fig/id_class_html_browser.png)



idとclassによる指定は，CSSやJavaScriptで重要な役割を果たします。