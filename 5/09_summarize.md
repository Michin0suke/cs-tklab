* [←教材管理システム](http://cs-tklab.na-inet.jp/phpdb/Chapter5/system8.html)
* [ホーム](http://cs-tklab.na-inet.jp/phpdb/index.html)
* [教材の消去→](http://cs-tklab.na-inet.jp/phpdb/Chapter5/system9.html)

# 共通する機能をまとめる

------

## common/common.phpの作成と既存PHPスクリプトの書き換え

セッション配列($_SESSION)の値をチェックしてログイン済みかどうかを判断する処理はtop_page.php, learning.phpでも同一です。従ってこれは一つのPHPファンクション`login_check`にまとめて共通化しましょう。ついでに，MySQLサーバに接続する処理(`dbconnect.php`)の機能も，`common/common.php`にまとめてしまいます。

まず，現在の`challenge`フォルダ(ディレクトリ)の下に`common`フォルダを作り，`dbconnect.php`の内容をコピペし，その下にログインチェック関数を作ります。

common/common.php

[![img](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/common_php.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/common_php.png)



次に，`dbconnect.php`を呼び出しているPHPスクリプトを，全て`common/common.php`を呼び出すように書き換えます。



##### 変更前

[![img](09_summarize.assets/dbconnect2common_before.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/dbconnect2common_before.png)

　　↓ 　3行目を変更 　　↓

##### 変更後

[![img](09_summarize.assets/dbconnect2common.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/dbconnect2common.png)



下記のPHPスクリプトを上記のように変更して下さい。

* index.php
* entry.php
* check.php



`top_page.php`と`learning.php`はログインチェック部分もlogin_check関数を使って共通化できるので，次のように書き換えます

#### top_page.php

[![img](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/top_page_php_common.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/top_page_php_common.png)

#### learning.php

[![img](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/learning_php_common.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/learning_php_common.png)

## セキュリティの確保

これ以降は，共通する機能や変数の操作は全て`common/commo.php`に記述し，なるべくPHPスクリプトの行数を少なく抑えるようにしていきます。逆に，重要かつセキュリティ上保護すべき機能も書きこまれることになりますので，これを放置してはいけません。以下，Webアプリケーションのセキュリティを確保する手続きを説明します。

### dbconnect.phpを削除する

今まで使ってきた`dbconnect.php`を放置していてはセキュリティ上問題がありますのでこれを削除します。保存したければブラウザからアクセスできないフォルダにコピーしておいて下さい。

### common/common.phpのアクセスを制限する

`common/common.php`もWebブラウザからダイレクトにアクセスできないようにしましょう。そのためには

1. htdocsより上の階層のフォルダに置いておく
2. Apacheのアクセス制限機能を使う

という2つの方法が使えます。ここでは2について解説します。



Apacheの設定にもよりますが，ユーザー側で追加のアクセス制限をかける時には，同じフォルダに`.htaccess`というテキストファイルを作成し，指定したファイル・フォルダのアクセスを制限することができます。例えば`common/common.php`を外部からアクセスできないようにするためには`.htaccess`を



[![img](09_summarize.assets/htaccess.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/htaccess.png)



とします。実際これでアクセスできなくなったかどうかをhttp://localhost/challenge/common/common.phpにアクセスして確認して下さい。

[![img](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/htaccess_deny.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/htaccess_deny.png)

と出れば制限されていることが確認できます。

------

* [←教材管理システム](http://cs-tklab.na-inet.jp/phpdb/Chapter5/system8.html)
* [ホーム](http://cs-tklab.na-inet.jp/phpdb/index.html)
* [教材の消去→](http://cs-tklab.na-inet.jp/phpdb/Chapter5/system9.html)

Copyright (c) 2014-2017 幸谷研究室 @ 静岡理工科大学 All rights reserved.
Copyright (c) 2014-2017 T.Kouya Laboratory @ Shizuoka Institute of Science and Technology. All rights reserved.