* [←提出課題の内容変更](http://cs-tklab.na-inet.jp/phpdb/Chapter5/system11.html)
* [ホーム](http://cs-tklab.na-inet.jp/phpdb/index.html)
* [システムの改良→](http://cs-tklab.na-inet.jp/phpdb/Chapter5/system13.html)

# 全体の提出内容の表示

------

## 全体提出状況の表示: submission.php

受講全員の課題提出状況を一覧できると教師としては非常に便利です。既に`task.php`では受講生毎の提出物の一覧を表示させていましたが，ここではその制限を外し，下記のように表示するページを作成します。

[![img](13_list_of_contents.assets/system12-1.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/system12-1.PNG)

PHPスクリプト： submission.php

[![img](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/submission_php_common.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/submission_php_common.png)



------

## 解説

### 提出者の識別表示

リレーショナルベースデータベースの機能として，異なるテーブルに共通するカラムがある場合，それを元に互いの情報を組み合わせた新たなテーブルを作成することができるようになります。例えばこの講義支援システムの[データ構造](http://cs-tklab.na-inet.jp/phpdb/Chapter5/system2.html)の場合，`task`テーブルの`member`フィールドは，`member`テーブルの`id`を格納しているので，共通のものになっています。この場合，その例として，今回表示している提出者の表示が挙げられます。



[![img](13_list_of_contents.assets/system12-3.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/system12-3.PNG)



SQL文だけで上記のような複数テーブルを組み合わせたテーブルを作成することが可能ですが，以下ではこの手法は使っていません。チャレンジしてみたい人は`JOIN`をキーワードにいろいろ検索してこの手法を編み出してみて下さい。

------

ここでは次のような手順で二つのテーブルの情報を組み合わせた表示を行っています。

1. 18～19行目で`task`テーブルに登録されている`member`カラムの情報を引っ張り出す
2. 27行目から始まる`while文`による全件表示内で，29～31行目に`member`テーブルの情報を引き出す`SELECT文`を発行
3. `member`テーブルの`name`情報を38行目で表示



これを図解したのが下記になります。



[![img](13_list_of_contents.assets/system12-4.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/system12-4.png)



------

* [←提出課題の内容変更](http://cs-tklab.na-inet.jp/phpdb/Chapter5/system11.html)
* [ホーム](http://cs-tklab.na-inet.jp/phpdb/index.html)
* [システムの改良→](http://cs-tklab.na-inet.jp/phpdb/Chapter5/system13.html)

Copyright (c) 2014-2017 幸谷研究室 @ 静岡理工科大学 All rights reserved.
Copyright (c) 2014-2017 T.Kouya Laboratory @ Shizuoka Institute of Science and Technology. All rights reserved.