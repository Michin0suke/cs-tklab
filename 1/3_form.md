# フォーム

------

## フォームとは

フォームとは，ブラウザからサーバに対して情報の受け渡すことを目的としたHTMLの機能です。静的なHTMLファイルとして記述する時には，`<form>～</form>`の中に，次のような入力用の項目を入れ込んで構築していきます。

------

### フォームの例

テキストボックス: <input type="text">

パスワード: <input type="password">

ボタン: <button>単なるボタン</button>

ラジオボタン: <input type="radio" name="hoge">男<input type="radio" name="hoge">女

チェックボックス: <input type="checkbox">赤<input type="checkbox">青<input type="checkbox">緑<input type="checkbox">黄

リストメニュー: <select><option>北海道</option><option>青森県<option></select>

ファイルメニュー: <input type="file">

テキストエリア: 

<textarea cols="30" rows="5" placeholder="自由に意見を記述してください。"></textarea>

送信(submit): <input type="submit" value="送信">

リセット: <input type="reset" value="リセット">



------

上記のフォームに内容を記入・選択して「送信する」ボタンをクリックすると，`form`タグの`action`属性に指定したURLに，このフォームの記述内容が送信されます。

## フォームに使用する要素

### form要素: `<form ... > ... </form>`

* action属性

  入力値の送信場所を指定します。

* method属性

  送信方法を指定します。この方法には2種類あり，入力値に`get`か`post`を入力します。

### form要素内に記述する入力用の要素: `<input ... />`

フォーム内に記述する入力用のinput要素には次の属性を指定します。

* type属性

  フォームの入力方式を決定することができます。入力方式としては下記のものがあります。テキストボックスパスワードボックスボタンラジオボタンチェックボックスリストメニューファイルメニューテキストエリアまた，HTML5で追加された入力方式として，下記のものがあります。日付(date)ローカル時間(datetime-local)時刻(time)月(month)週(week)カラー(色, color)番号(number)スライドバー(範囲, range)URL(url)メールアドレス(email)検索(search)

* name属性

  フォームの入力内容のやり取りを行う際の目印になります。 特にラジオボタンではnameを同じにすることでグループ化しているので注意が必要になります。

* value属性

  初期入力値を設定します。 選択するチェックボックスやラジオボタンではここで入力しなければ入力値が空になってしまいます。

### ボタン用のinputタグ

入力内容に対してアクションを起こすためのボタンとして使います。属性値`type="値"`を設定してボタンの用途を決定します。

* `type="submit"`

  所謂「送信」ボタンです。フォーム内容を送信する際に使用します。

* `type="reset"`

  入力内容をキャンセルし初期化します。

inputタグを使わない入力要素としては下記のものがあります。inputタグとは異なる属性値を使用する場合があります。

### textarea要素

テキストエリアの入力方式を使うことができます。

### select要素

ファイルメニューの入力方式を使うことができ，メニュー欄を作成する際にはoption要素が必要となります。

------

## form要素内に記述する入力用の要素の例

以下では今後使用する機会の多いform要素内の入力用要素の具体例を見ていくことにしましょう。入力用要素はformタグに囲まれていなくても表示できますが，そのままでは入力された値を送信することはできません。下記の例は説明のため，form要素はカットしてあります。

### テキストボックス: `<input type="text" ... />`

短い分を入力する基本的な形式で，`type`を省略することも可能です。

#### 使用属性

* `type="text"`：テキストボックス形式であることを示す
* `name`：送信の目印となる名前を決める
* `value`：初期入力値を設定する
* `size`：テキストボックスの表示サイズを設定する
* `maxlength`：入力最大文字数を設定する

#### テキストボックスの例



HTMLファイル

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>sample1</title>
</head>
<body>
    <input type="text" name="sample" value="サンプル" size="30" maxlength="255">
</body>
</html>
```

------

### パスワード: `<input type="password" ... />`

基本的にはテキストボックスと同じ機能を持ちますが，入力された文字列を隠すことができます。

#### パスワードの例



HTMLファイル

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>パスワードサンプル</title>
</head>
<body>
    <h1>パスワードサンプル</h1>
    <input type="password" name="sample" size="30" maxlength="255">
</body>
</html>
```

------

### ボタン: `<input type="button" ... />`

送信(submit)，リセット(reset)とも異なる，単なるボタンです。

#### ボタンの例

HTMLファイル

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>ボタンのサンプル</title>
</head>
<body>
    <h1>ボタンのサンプル</h1>
    ボタン: <input type="button" name="sample" value="単なるボタン">
</body>
</html>
```

------

### ラジオボタン:`<input type="radio" ... />`

選択肢を一つだけ選んでほしい時に使用します。

#### 使用属性

* `type="radio"`：ラジオボタン形式であることを示す
* `name`：送信の目印となる名前を決める
* `value`：入力値を設定する(画面表示はされません)
* `checked`: 最初から選択された状態にする

#### ラジオボタンの例

　Sample01
　Sample02

HTMLファイル

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>sample2</title>
</head>
<body>
    <ul>
        <li>input type="radio" name="sample" value="sample" checked>Sample01</li>
        <li>input type="radio" name="sample" value="sample">Sample02</li>
    </ul>
</body>
</html>
```

------

### チェックボックス:`<input type="checkbox" ... />`

チェックをした複数の内容を送信することができます。

#### 使用属性

* `type="checkbox"`：チェックボックス形式であることを示す
* `name`：送信の目印となる名前を決める
* `value`：入力値を設定する(画面表示はされません)
* `checked`:最初から選択された状態にする

#### チェックボックスの例

sample1 sample2 sample3

HTMLファイル

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>sample3</title>
</head>
<body>
    <label><input type="checkbox" name="sample" value="sample">sample1</label>
    <label><input type="checkbox" name="sample" value="sample">sample2</label>
    <label><input type="checkbox" name="sample" value="sample" checked>sample3</label>
</body>
</html>
```



------

### セレクト: `<select ... > ～<option ... >～</option> ～ </select>`

ドロップダウンリストの中に示した選択肢から一つあるいは複数選ばせる際に使用します。

#### 使用タグと属性

タグ：`select`(囲まれた要素全体がリストボックスの内容になる)

* `name`：送信の目印となる名前を決める

タグ：`option`(ドロップダウンされる1項目を作る)

* `value`：入力値を設定

#### セレクトの例

 sample01 sample02 sample03 

HTMLファイル

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>sample4</title>
</head>
<body>
    <select>
        <option value="1">sample01</option>
        <option value="2">sample02</option>
        <option value="3">sample03</option>
    </select>
</body>
</html>
```

------

### ファイルメニュー: `<input type="file" ... />`

ローカルの既存ファイルを選択し，選択したファイルを送信することができるようにするメニューです。ファイルメニューを使う際は，form要素に`enctype`属性を必ず指定する必要があります。

使用属性 タグ：form

* `enctype="multipart/form-data"`：ファイルを送信する際には必ず必要な属性
  ファイルをコード化し，文字情報にする

タグ：input

* `type="file"`：ファイルを選択し，送信することができるようにする/li>
* `name`：送信の目印となる名前を決める
* `size`：フォームのサイズを指定する

#### ファイルメニューの例



HTMLファイル

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>sample5</title>
</head>
<body>
    <input type="file" name="sample">
</body>
</html>
```

------

### テキストエリア:`<textarea ...>～</textarea>`

縦方向(`rows`)と横方向(`cols`)をそれぞれ指定して長文を入力できるようにしたテキストボックスです。初期入力値を設定する場合は，

<textarea ...>この部分に初期入力値を入れる</textarea>

のように書いておきます。



#### 使用属性

* `name=""`：送信の目印となる名前を決める
* `cols=""`：属性値に数字を入れることでエリアの横の長さを決める
* `rows=""`：属性値に数字を入れることでエリアの縦の長さを決める

#### テキストエリアの例

HTMLファイル

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>sample6</title>
</head>
<body>
    <textarea name="sample" cols="50" rows="10" placeholder="入力してください。"></textarea>
</body>
</html>
```