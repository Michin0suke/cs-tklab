# ログアウト

------

## ログアウト: logout.php

`ログアウト`リンクをクリックするか，ログイン条件を満足しなくなった時にはこの`logout.php`に飛ばされ，下記のような表示を得ます。



[![img](07_logout.assets/system7-1.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/system7-1.PNG)



PHPスクリプト：logout.php

```php
<?php
session_start();
session_unset();
?>

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>ログアウト</title>
</head>
<body>
    <p>ログアウトが実行されました。</p>
    <p><a href="index.php">ログインページへ</a></p>
</body>
</html>
```



------

## 解説

この講義支援システムのログイン情報はセッションの引継ぎによって保たれています。従って，ログアウトの際に必要な処理は，セッションによる情報の引継ぎを止めて破棄することです。これは`session_unset`ファンクションを利用することで実現できます。

このページでは説明のためにログアウトが完了したことを表示していますが，`header('Location: ○○')`を使用することで，ログアウト結果の確認を省略し，例えば`index.php`に飛ばすことも可能です。

表示を省略したlogout.php

```php
<?php
session_start();
session_unset();
header('Location: index.php');
```



