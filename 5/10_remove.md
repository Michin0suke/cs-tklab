# 教材の消去

------

## 教材の消去: delete.php

[教材管理のところでも説明しました](http://cs-tklab.na-inet.jp/phpdb/Chapter5/system8.html#delete)が，削除すべき教材のidが送られてくるようになっているので，これを使ってDELETE命令で消去しています。セキュリティ的には考えるところの多い仕様なので，改良案を思いついたら提案して下さい。

PHPスクリプト: delete.php

```php
<?php
    session_start();
    require('common/common.php');

    // ログインチェック
    login_check($member, $db);

    if (isset($_REQUEST['id'])) {
        $id = $_REQUEST['id'];

        //投稿を検査する
        $sql = 'SELECT * FROM learning WHERE id = '.sanitize($db, $id);
        $record = mysqli_query($db, $sql) or die(mysqli_error($db));
        $table = mysqli_fetch_assoc($record);
        if ($table['id'] == $_REQUEST['id']) {
            // 消去
            $sql = 'DELETE FROM learning WHERE id = '.sanitize($db, $id);
            mysqli_query($db, $sql) or die(mysqli_error($db));
        }
    }
    header('Location: learning.php');
    exit();
```



------

## 解説

登録データの消去はデータベースの取扱いで一番気を付けなければならない操作です。必ず12行目のようにログインチェックを行って消去する権限のあるユーザーのみ実行できるようにして下さい。

さもなければ，この`delete.php`にアクセスできるすべてのユーザがあらゆるデータを消去できるようになってしまいます。

削除はいったん実行してしまうと取り返しがつきません。できれば実行前に「削除します。よろしいですか？」という確認のためのメッセージを出すようにプログラミングしましょう。

