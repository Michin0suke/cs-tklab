* [←教材の消去](http://cs-tklab.na-inet.jp/phpdb/Chapter5/system9.html)
* [ホーム](http://cs-tklab.na-inet.jp/phpdb/index.html)
* [提出課題の内容変更→](http://cs-tklab.na-inet.jp/phpdb/Chapter5/system11.html)

# 課題提出システム

------

## 課題提出システム: task.php

提出される課題を受け取ると共に，今まで提出した課題の一覧も表示するのがこの課題提出システム`task.php`です。下記のようなページを作っていきます。

[![img](11_description_submission_system.assets/system10-1.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/system10-1.PNG)

PHPスクリプト：task.php

[![img](11_description_submission_system.assets/task_php_common.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/task_php_common.png)



[注意！] 教材管理システム`learning.php`と同じように，フォルダ`challenge`の中に`task_folder`を作るのも忘れないようにして下さい。ここに課題ファイルが保存されます。

------

## 解説

基本的な構造は`learning.php`と同じですが，表示する情報が違うため相違点を解説します。

### 課題登録に関しての相違点

[![img](11_description_submission_system.assets/system10-3.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/system10-3.PNG)

登録内容に受講生が一言述べるためのコメント欄が追加されています。これは，課題を解く過程で詰まったところを質問したりするためのものです。

コメントを書くことによって，課題を受け取る管理者（教師）側は，課題の難易度などをを判断することができますし， 提出する受講生側も，課題の内容を振り返ることによって自分に足りない点に気づくことができます。

------

### 表示に関しての相違点

課題提出をしている場合は下のような表示となります。

[![img](11_description_submission_system.assets/system10-4.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/system10-4.PNG)

このページは受講生個々人の提出状況を確認することができるようになっています。そのため，受講生ごとに自分が提出した課題一覧を表示することが必要になります。このため，登録内容を全件取得していた`learning.php`とは異なり，受講生個人を特定した選択をするため，SELECT命令を発行する際にはWHEREを使用することで，受講生idを選択して取得しています。

PHPスクリプトの43～48行目の内容がその部分にあたり，`member`テーブルの中から`$member`に格納されている受講生idを利用して登録者を特定し，提出課題一覧を取得しています。



[![img](11_description_submission_system.assets/task_php_common_l43-l48.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/task_php_common_l43-l48.png)



------

* [←教材の消去](http://cs-tklab.na-inet.jp/phpdb/Chapter5/system9.html)
* [ホーム](http://cs-tklab.na-inet.jp/phpdb/index.html)
* [提出課題の内容変更→](http://cs-tklab.na-inet.jp/phpdb/Chapter5/system11.html)

Copyright (c) 2014-2017 幸谷研究室 @ 静岡理工科大学 All rights reserved.
Copyright (c) 2014-2017 T.Kouya Laboratory @ Shizuoka Institute of Science and Technology. All rights reserved.