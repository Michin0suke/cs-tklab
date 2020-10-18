# 提出課題の内容変更

------

## 提出課題の内容変更: change.php

下記のように登録した課題を変更するリンクから呼び出されるページがこの`change.php`です。

### [task.phpページ](http://cs-tklab.na-inet.jp/phpdb/Chapter5/system10.html)から登録内容変更ページへのリンク

[![img](12_change_contents.assets/task_php_common_view.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/task_php_common_view.png)

下記のフォームのように，登録された課題ファイル情報を呼び出し，題名，コメント，課題ファイルそれぞれを個別に変更する必要があるため，登録した課題の情報を変更する方が面倒になります。

### 登録内容の変更ページ

[![img](12_change_contents.assets/change_php_common_view.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/change_php_common_view.png)



PHPスクリプト：change.php

```php
<?php
session_start();
require('common/common.php');

// ログインチェック
login_check($member, $db);

$messages = [];

if(!empty($_POST)) {
    if(!empty($_FILES)) {
        // 拡張子判別
        $messages[] = 'ファイル更新';
        $file = mb_convert_kana($_FILES['file']['name'], 'a', 'UTF-8');

        if(preg_match("/\.\w{4}\z/", $file))
            $ext = substr($file, -5);
        else if (preg_match("/\.\w{3}\z/", $file))
            $ext = substr($file, -4);

        $sql = sprintf('UPDATE task SET file="%s", change_name="%s", modified="%s" WHERE id=%d',
            sanitize($db, $_FILES['file']['name']),
            sanitize($db, $_SESSION['chan'].$ext),
            sanitize($db, date('Y-m-d')),
            sanitize($db, $_SESSION['task'])
        );
        mysqli_query($db, $sql) or die(mysqli_error($db));

        // ファイル登録
        $filepath = './task_folder/'.$_SESSION['chan'].$ext;
        move_uploaded_file($_FILES['file']['tmp_name'], $filepath);
        $messages[] = 'ファイル更新終了';
    } else {
        $messages[] = 'データ更新';
        $sql = sprintf('UPDATE task SET name="%s", word="%s", modified="%s" WHERE id=%d',
            sanitize($db, $_POST['name']),
            sanitize($db, $_POST['word']),
            sanitize($db, date('Y-m-d')),
            sanitize($db, $_SESSION['task'])
        );
        mysqli_query($db, $sql) or die(mysqli_error($db));
    }
    $error['task'] = 'blank';
}

// 投稿を取得する
if (isset($_REQUEST['id'])) {
    $_SESSION['task'] = $_REQUEST['id'];
    $_SESSION['chan'] = "exc".$_SESSION['task'];

    $sql = 'SELECT * FROM task WHERE id = '.sanitize($db, $_SESSION['task']);
    $record = mysqli_query($db, $sql) or die(mysqli_error($db));
    $table = mysqli_fetch_assoc($record);
}
?>

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>課題の編集</title>
</head>
<body>
    <?=implode('<br>', $messages)?>
    <?php if($table['id'] == $_REQUEST['id']): ?>
        <h2>編集欄</h2>
        <form method="post">
            <table border="1">
                <tr>
                    <th>題名</th>
                    <td colspan="3">
                        <input type="text" name="name" value="<?=htmlspecialchars($table['name'], ENT_QUOTES)?>" size="80">
                    </td>
                </tr>
                <tr>
                    <th>コメント</th>
                    <td colspan="3">
                        <textarea name="word" cols="80" rows="5"><?=htmlspecialchars($table['word'], ENT_QUOTES)?></textarea>
                    </td>
                </tr>
            </table>
            <input type="hidden" name="file_update" value="yes">
            <input type="submit" value="変更する">
        </form>
        <form method="post" enctype="multipart/form-data">
            <p>登録済ファイル: <?=htmlspecialchars($table['file'], ENT_QUOTES)?></p>
            <p>変更ファイル: <input type="file" name="file"></p>
            <input type="hidden" name="file_update" value="yes">
            <br>
            <input type="submit" value="変更する">
        </form>
    <?php endif ?>
    <hr>
    <a href="task.php">←課題提出ページ</a>
</body>
</html>
```





------

## 解説

### 編集欄への変更前情報の表示

登録内容を変更するため，変更前の情報を呼び出してあらかじめフォームの入力欄に表示します。こうすることで，変更点が明確になりますし，変更する必要のない項目はそのままにしておけます。

55～65行目では，`task.php`から送られてきた，変更すべき部分の提出課題の`id`を使ってデータベースから情報を引き出しています。



[![img](12_change_contents.assets/change_php_common_l55-l65.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/change_php_common_l55-l65.png)



フォームの入力要素であるテキストボックスの場合，引き出した内容を`input`タグの`value`属性として与えておくことで，表示することができます。テキストエリアの場合は`textarea`タグで挟み込んだ部分に初期文字列の設定ができます。



[![img](12_change_contents.assets/change_php_common_l71-l81.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/change_php_common_l71-l81.png)



------

### 登録内容の変更

ここで気を付けることは`change_name`の設定です。

`task.php`で登録したファイル名と被らないよう，`$_SESSION['chan']`に 前回までとは違うファイル名を作り，これを新たな登録ファイル名としています。



[![img](12_change_contents.assets/change_php_common_l33.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/change_php_common_l33.png)



このファイル名変更の仕様については改善の余地があります。「別に同じファイル名でも上書きすればいいじゃないか」と考える人は，そのようにこの`change.php`を書き換えてみて下さい。

