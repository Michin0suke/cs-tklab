# 教材管理システム

------

## 教材管理システム: learning.php

教材や課題を登録するページを作ります。最終的には管理者（教師）のみが教材の登録や変更が可能なように制限をかけますが，ここではログインユーザーかどうかの確認のみ行っています。

[注意！] 教材ファイルを保存するためのフォルダ`learning_folder`を`challenge`に作るのも忘れないようにして下さい。これがないと教材登録が正常にできません。



[![img](08_description_management_system.assets/system8-1.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/system8-1.PNG)



プログラム：learning.php

```php
<?php
session_start();
require('dbconnect.php');

if(isset($_SESSION['id']) && $_SESSION['time'] + 3600 > time()) {
    // ログイン状態
    $_SESSION['time'] = time();
    $sql = 'SELECT * FROM member WHERE id = '.sanitize($db, $_SESSION['id']);
    $record = mysqli_query($db, $sql) or die(mysqli_error($db));
    $member = mysqli_fetch_assoc($record);
} else {
    // ログインしていない
    header('Location: index.php');
    exit();
}

if(!empty($_POST)) {
    // 拡張子判別
    $file = mb_convert_kana($_FILES['learning']['name'], 'a', 'UTF-8');
    if(preg_match("/\.\w{4}\z/", $file))
        $ext = substr($file, -5);
    else if (preg_match("/\.\w{3}\z/", $file))
        $ext = substr($file, -4);

    // 登録処理
    if(!empty($_POST['file_name']) && !empty($_FILES['learning']['name'])) {
        $sql = sprintf('INSERT INTO learning SET member=%d, name="%s", file="%s", change_name="%s", created="%s"',
            sanitize($db, $member['id']),
            sanitize($db, $_POST['file_name']),
            sanitize($db, $_FILES['learning']['name']),
            sanitize($db, $_SESSION['change_name'].$ext),
            sanitize($db, date("Y-m-d"))
        );
        mysqli_query($db, $sql) or die(mysqli_error($db));

        // ファイル登録
        $filepath = './learning_folder/'.htmlspecialchars($_SESSION['change_name'], ENT_QUOTES);
        move_uploaded_file($_FILES['learning']['tmp_name'], $filepath.$ext);
    } else {
        $error['learning'] = 'blank';
    }
}

// ページの取得
$sql = 'SELECT * FROM learning ORDER BY id ASC';
$recordSet = mysqli_query($db, $sql) or die(mysqli_error($db));
?>

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>教材管理ページ</title>
    <style>
        #red { color: red; }
    </style>
</head>
<body>
    <h1>教材管理ページ</h1>
    <p>ログインユーザ: <?=htmlspecialchars($member['name'], ENT_QUOTES)?></p>
    <hr>
    <p>公開されている情報</p>
    <?php
        $i = 1;
        while($tables = mysqli_fetch_assoc($recordSet)) {
    ?>
    <table border="1">
            <tr>
                <th width="20"><?=$i?></th>
                <th colspan="3"><?=htmlspecialchars($tables['name'], ENT_QUOTES, 'utf-8')?></th>
            </tr>
            <tr>
                <td colspan="2">
                    <a href="learning_folder/<?=htmlspecialchars($tables['change_name'], ENT_QUOTES, 'utf-8')?>">
                        <?=htmlspecialchars($tables['file'], ENT_QUOTES, 'utf-8')?>
                    </a>
                </td>
                <td width="180">公開日時: <?=htmlspecialchars($tables['created'], ENT_QUOTES, 'utf-8')?></td>
                <td width="40">
                    <a href="delete.php?id=<?=$tables['id']?>">消去</a>
                </td>
            </tr>
    </table>
    <br>
    <?php
        $i++;
        }
        $_SESSION['change_name'] = $i . "-" . date("Ymd");
        if ($i == 1) echo '<p>登録されている情報はありません</p>';
    ?>
    <hr>
    <form method="post" enctype="multipart/form-data">
        <p>アップロードファイルの選択: </p>
        <table border="1">
            <tr><th>題名</th><td><input type="text" name="file_name" size="30"></td></tr>
            <tr><th>file</th><td><input type="file" name="learning" size="50"></td></tr>
        </table>
        <?php
            if(!empty($error['learning']) && $error['learning'] === 'blank') {
                var_dump($error);
                echo '<p id="red">※題名とファイルを確実に選択してください。</p>';
            }
        ?>
        <input type="submit" value="アップロード">
    </form>
    <hr>
    <p><a href="top_page.php">トップに戻る</a></p>
    <a href="logout.php">ログアウト</a>
</body>
</html>

```



------

## 解説

### 6～19行：ログインの判別

すでにログインしているかどうかの判別を行っています。この部分はトップページ([top_page.php](http://cs-tklab.na-inet.jp/phpdb/Chapter5/system6.html))と全く同じ処理を行っています。

### 50・51行：ページの取得

ここでは教材情報を登録している`learning`テーブルの内容を`ORDER BY id ASC`を使ってidカラムの昇順(`AUTO_INCREMENT`)を使用しているので登録順)に並べ替えて全件を変数`$recordSet`に格納しています。



[![img](08_description_management_system.assets/system8-3.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/system8-3.PNG)



------

### 67～90行：登録内容の表示

既に教材を登録していた場合，表示は下記のようになります。



[![img](08_description_management_system.assets/system8-4.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/system8-4.PNG)



表示内容の左上には番号があります。左下には消去があるので登録内容は消去することができることがわかります。

データベースに登録されたidをそのまま表示してしまうと，例えば教材の消去をした場合にはそのid番号が欠番となり，番号が飛び飛びになってしまうことがありますのでよろしくありません。従ってここでは70行目に変数`$i`を設定し，繰り返しの最後である87行目で1ずつプラスして番号飛びが発生しないようにしています。この部分は`for文`を使って記述することも可能です。

------

登録された教材へのダウンロード用リンクの表示は79～80行目で行っています。ここで注意してほしいのは，データベースに登録してあるファイル名(ここでは`$tables['file']`にあたる)と，実際にリンクしているファイル名(`$tables['change_name']`)が異なっている点です。



[![img](08_description_management_system.assets/system8-5.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/system8-5.PNG)



この理由は，登録ファイル名が日本語交じりの全角文字列であった場合でも対応できるようにするためです。登録した教材ファイルを保管するのはサーバ側になりますが，全角文字ファイル名を正しく扱えなかったり，サーバへの転送時に文字化けしてしまう可能性があり，結果として「登録したのに表示させることができない」という状況になりかねません。

このような事態を避けるため，たとえ登録するファイル名と一対一対応の半角ASCII文字範囲のファイル名を新しく付け直しています。これが`$tables['change_name']`で，89行目に変更名を登録順である`$i`と現在の年月日時刻である`date`を繋ぎ合わせて作られています。



[![img](08_description_management_system.assets/system8-6.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/system8-6.PNG)



------

82行目には`消去`のリンクがあります。教材の消去を行うPHPスクリプト`delete.php`に`?id=xxx`を付加したリンクを作ることで，このリンクをクリックした後で実行される`delete.php`では`$_REQUEST['id']`を使用することができるようになります。ここに格納されている教材情報のid(`=xxx`)を使ったDELETE命令を実行することで指定された情報を消去することができるようになります。



[![img](08_description_management_system.assets/system8-7.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/system8-7.PNG)



------

90行目はデータベースに教材登録情報の有無をチェックしています。`if`の判定内容は「データベースから引き出し，格納した変数が存在するか」でも構いませんが，ここでは繰り返しの番号である`$i`を利用し，「1度も繰り返しがなかった場合」で判定しています。



[![img](08_description_management_system.assets/system8-8.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/system8-8.PNG)



------

### 21～48行

21～25行では登録するファイルの拡張子を判別しています。登録教材の元のファイル名のまま登録することができるならば判別の必要はありませんが，前述した通り変更していますので，拡張子をそのまま残す目的で判別を行っています。

拡張子の判別には正規表現を利用しています。ファイル名の文字列の後ろから4または5文字が`.(ドット)w{○}(○内は文字数)` であるかどうかを調べ，その部分を拡張として取り出して変数`$ext`に格納しています。これを教材ファイルの登録時に利用します。



[![img](08_description_management_system.assets/system8-9.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/system8-9.PNG)



------

31～48行目で，見慣れない処理を行っている所ではファイル名の変更を行っています。

36行目では変更後のファイル名をデータベースに登録しています。そのため，89行目で新しく作られた名前と拡張子`$ext`をくっつけることで，元のファイルと同じ拡張子を持ったファイル名になります。こうして転送した元のファイル名と齟齬が発生しないようにしている訳です。



[![img](08_description_management_system.assets/system8-10.PNG)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/system8-10.PNG)



