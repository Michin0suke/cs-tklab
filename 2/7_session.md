# セッション

------

セッションとは，PHPシステム側で用意される時限付きの状態記憶機能です。セッションが有効である間は固有の変数（セッション配列）である`$_SESSION`を使うことができ，どのPHPスクリプトであってもセッションを有効にすればこの`$_SESSION`に記憶された値を共有し，読み書きすることができるようになります。これによって，例えば一度ログインしたユーザかどうかの情報をどのページでも確認することが可能になります。

------

## セッションを使用する際に必要な注意

* セッションを使用するページでは，必ず最初に`session_start`ファンクションを使用し，初期化処理を行う必要があります。
* セッションが開始されると，終了処理が行われるまでセッション配列の情報は保持されます。使用時には必ず次の項目であるセッションの終了処理を行って下さい。さもなければ，意図しないページを埋め込まれた時に，セッション配列を通じて情報が漏えいする可能性が出てきます(セッションハイジャック)。セッションの開始と終了は必ずセットにして実行して下さい。

## セッションの利用例

セッション配列`$_SESSION`に値を格納し，別ページでも利用できること確認しましょう。ここで使用するファイルと，セッション変数の共用の様子を示したものが下記の図になります。一通り実行した後で，`$_SESSION['word']`に格納された値がどのように共通化されているのか，もう一度確認して下さい。

[![img](7_session.assets/session_php.svg)](http://cs-tklab.na-inet.jp/phpdb/Chapter2/fig/session_php.svg)

### セッションの開始

まず，次のフォームから値を入力します。

HTMLファイル：session_form.html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>セッション</title>
</head>
<body>
  <form action="session_start.php" method="post">
    <label>
      継続内容:
      <input type="text" name="word">
    </label>
    <input type="submit" value="submit">
    <input type="reset" value="reset">
  </form>
  <p><a href="index.html">return to index</a></p>
</body>
</html>
```



画面の表示

[![img](7_session.assets/image7-1.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter2/fig/image7-1.PNG)



フォームから受け取ったデータ`$_POST['word']`をセッション配列`$_SESSION['word']`に格納するスクリプトは次のようになります。

PHPスクリプト：session_start.php

```php
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>セッション2</title>
</head>
<body>
    <?php session_start() ?>
    <p>セッションスタート</p>
    <?php $_SESSION['word'] = htmlspecialchars($_POST['word'], ENT_QUOTES) ?>
    <p>現在の継続情報は[ <?=$_SESSION['word']?> ]と登録されています。</p>

    <p><a href="session_follow.php">継続の確認</a></p>
</body>
</html>
```



### セッション変数の利用

`$_SESSION['word']`に入力された値を表示して確認し，ページ下のリンクをクリックして別のPHPスクリプト`session_follow.php`に飛んでみましょう。

画面の表示

[![img](7_session.assets/image7-2.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter2/fig/image7-2.PNG)



前のページと同じ値が`$_SESSION['word']`から取得できるか確認するPHPスクリプトが下記になります。

PHPスクリプト：session_follow.php

```php
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>セッション3</title>
</head>
<body>
    <?php session_start() ?>
    <p>セッション継続中です。</p>
    <p>現在の継続情報は[ <?=$_SESSION['word']?> ]と登録されています。</p>

    <p><a href="session_end.php">セッション終了へ</a></p>
</body>
</html>
```





同じ値が入っていることを確認したら，ページ下のリンクからセッションを終了させるPHPスクリプトに飛びます。

画面の表示

[![img](7_session.assets/image7-3.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter2/fig/image7-3.PNG)



### セッションの終了

セッションを終了するには`session_unset()`，あるいは`session_destroy()`ファンクションを使います。

`session_unset関数`は全てのセッションを終了させます。セッション配列の個別のインデックスを消去するには`unset($_SESSION[インデックス])`とします。

PHPスクリプト：session_end.php

```php
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>セッション4</title>
</head>
<body>
    <?php
        session_start();
        session_unset();
    ?>
    <p>現在の継続情報を終了しました。</p>
</body>
</html>
```





画面の表示

[![img](7_session.assets/image7-4.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter2/fig/image7-4.PNG)