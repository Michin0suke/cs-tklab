# 練習問題

------

[第1章の練習問題](http://cs-tklab.na-inet.jp/phpdb/Chapter1/lesson1.html)で制作した登録フォームの入力を受け取って表示するPHPスクリプトを制作しましょう。受け渡す方法(GETかPOSTか）や，フォーム入力データを受け渡すPHPスクリプトファイル名（～.php）の指定は適切なものを自分で指定して下さい。



[![img](8_test.assets/lesson2-1.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter2/fig/lesson2-1.PNG)



画面の表示

[![img](8_test.assets/lesson2-2.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter2/fig/lesson2-2.PNG)



------

分からないところは最初からあきらめずに自分で調べましょう。 どこを見ればいいか分からないときは下のヒントを見ましょう。



# 解答例

------

```php
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>受け取り</title>
</head>
<body>
    <p>name: <?=htmlspecialchars($_REQUEST['name'], ENT_QUOTES)?></p>
    <p>sex: <?=htmlspecialchars($_REQUEST['sex'], ENT_QUOTES)?></p>
    <p>color: <?=htmlspecialchars($_REQUEST['color'], ENT_QUOTES)?></p>
    <p>pref: <?=htmlspecialchars($_REQUEST['pref'], ENT_QUOTES)?></p>
    <p>comments: <?=htmlspecialchars($_REQUEST['comments'], ENT_QUOTES)?></p>
</body>
</html>
```




