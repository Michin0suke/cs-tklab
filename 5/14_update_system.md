# システムの改良

------

## 管理者のチェック機構

[システム構成図](http://cs-tklab.na-inet.jp/phpdb/Chapter5/system1.html#structure_figure)のところで解説したように，このシステムでは

* 学習教材のアップロード・消去(`learning.php`, `delete.php`)
* (受講生レポートの)提出状況の一覧(`submission.php`)

が管理者（教師）のみがアクセスできる機能として用意されています。従って，ログインしたユーザーが管理者かどうかを確認するための機構が必要になります。



そこで，管理者は特定の登録ユーザーを一人だけ指定しておき，その管理者ユーザーだけがこれらの機能にアクセスできるような仕組みを導入することにします。

### common.phpの改良: 管理者ユーザーの指定

まず，10行目から23行目のように，`common/common.php`に管理者のユーザー名を`$admin_name`としてあらかじめ記入しておくことにします。そして，自動的にこのユーザーIDを`$admin_id`として保持しておくことにしましょう。

しかる後，管理者かどうかの確認を行う関数`admin_check`を51行目～58行目のように定義します。ここでは，`$admin_id`とセッション配列のユーザーIDが一致するかどうかで判断しています。

common/common.php

```php
<?php
// -----------
// 共通ファイル
// -----------

// データベース関連
$db = mysqli_connect('localhost', 'root', 'secret', 'test_db') or die('MySQLサーバに接続できませんでした。');
mysqli_set_charset($db, 'utf8');

// 管理者ID
    $admin_name = "admin";
    $admin_id = '';

    if($admin_id === ''){
        $sql = "SELECT id FROM member WHERE name = '$admin_name'";
        $record = mysqli_query($db, $sql) or die(mysqli_error($db));
        $data = mysqli_fetch_assoc($record);
        $admin_id = $data['id'];
    }

// 関数群
function login_check(&$member, $db) {
    if(isset($_SESSION['id']) && $_SESSION['time'] + 3600 > time()) {
        // ログイン状態
        $_SESSION['time'];
        $sql = 'SELECT * FROM member WHERE id = '.mysqli_real_escape_string($db, $_SESSION['id']);
        $record = mysqli_query($db, $sql) or die(mysqli_error($db));
        $member = mysqli_fetch_assoc($record);
    } else {
        // ログインしていない場合
        header('Location: index.php');
        exit();

        //return login_failed
    }
}

function sanitize($db, $input) {
    return mysqli_real_escape_string($db, htmlspecialchars($input, ENT_QUOTES));
}

// 管理者かどうか
function admin_check($admin_id) {
    if(isset($_SESSION['id']) && $_SESSION['id'] === $admin_id)
        return TRUE;
    else
        return FALSE;
}
```



[![img](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/common_php_admin_ckeck.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/commom_php_admin_check.png)



### top_page.php: 提出状況一覧へのリンクを制限する

top_page.phpの一部

```php
<?php
session_start();
require('common/common.php');
// ログインしているかのチェック
login_check($member, $db);
?>

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>トップページ</title>
</head>
<body>
    <h1>チャレンジ最終問題</h1>
    <hr>
    <p>ログインユーザー: <?=htmlspecialchars($member['name'])?></p>
    <ul>
        <li><a href="learning.php">学習用教材リンク</a></li>
        <li><a href="task.php">課題提出リンク</a></li>
        <?php if(admin_check($admin_id)): // 管理者のみ?>
            <li><a href="submission.php">全体の提出状況</a></li>
        <?php endif ?>
    </ul>
    <hr>
    <a href="logout.php">ログアウト</a>
</body>
</html>
```





### learning.php: 学習教材のアップロード・消去機能の制限

管理者以外のユーザの表示[![img](14_update_system.assets/learning_php_admin_check_normaluser.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/learning_php_admin_check_normaluser.png)

管理者ユーザの表示[![img](14_update_system.assets/learning_php_admin_check_adminuser.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/learning_php_admin_check_adminuser.png)

learning.phpの一部(1/2)

```php
<?php
session_start();
require('common/common.php');

// ログインしているかのチェック
login_check($member, $db);

// 管理者のみ登録可
if(admin_check($admin_id)) {
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
                <?php if(admin_check($admin_id)): ?>
                    <td width="40">
                        <a href="delete.php?id=<?=$tables['id']?>">消去</a>
                    </td>
                <?php endif ?>
            </tr>
    </table>
    <br>
    <?php
$i++;
}
$_SESSION['change_name'] = $i . "-" . date("Ymd");
if ($i == 1) echo '<p>登録されている情報はありません</p>';

// 管理者のみ登録可
if(admin_check($admin_id)):
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
    <?php
endif
    ?>
    <hr>
    <p><a href="top_page.php">トップに戻る</a></p>
    <a href="logout.php">ログアウト</a>
</body>
</html>

```





### delete.php, submission.php: 実行そのものを制限する

念のため，`delete.php`, `submission.php`の冒頭部分にも`admin_check関数`によるif文を挿入し，実行を制限させておくと，ダイレクトにこれらのPHPスクリプトにアクセスされても実行できません。上記の例を参考に，この二つのPHPスクリプトにも管理者のみ実行可能なように制限をかけてみて下さい。これは課題とします。

## パスワードの暗号化

ユーザーのパスワードは厳重に保存しておく必要があります。たとえ管理者と言えども，パスワードそのものを直接見ることは好ましいことではありません。そこで，パスワードは暗号化したものを保存しておくように変更します。また，パスワードをフォームに入力する際にも`<input type="password">`タグを使って「********」のように伏字で表示させるようにしましょう。

PHP 5.5.0以上では`password_hash関数`を使って暗号化し，平文パスワードと一致するかどうかの確認は`password_verify関数`を用いて行うことが推奨されています。新規ユーザのフォームは`entry.php`，パスワードを保存するのは`check.php`で，ログイン時のパスワードチェックは`index.php`で行っていますので，この三つのPHPスクリプトを改良します。

### entry.php: フォーム入力時にパスワードを伏字にする

新規ユーザー登録時にパスワード入力部分を伏字にします。

entry.phpの変更箇所

[![img](14_update_system.assets/entry_php_encrypted_form.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/entry_php_encrypted_form.png)



新規ユーザー情報を入力する際に，下記のようにパスワード入力が伏字になることを確認しましょう。

[![img](14_update_system.assets/entry_php_encrypted_form_output.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/entry_php_encrypted_form_output.png)



### check.php: パスワードの暗号化と暗号化パスワードの登録

`entry.php`から受け取ったパスワードを`password_hash`で暗号化します。

check.phpの変更箇所

```php
<?php
session_start();
require('common/common.php');

if(!isset($_SESSION['join'])) {
    header('Location: entry.php');
    exit();
}

$sql = sprintf('SELECT count(*) FROM member WHERE name="%s"',
    sanitize($db, $_SESSION['join']['name'])
);
$record = mysqli_query($db, $sql) or die(mysqli_error($db));
$data = mysqli_fetch_row($record);
if($data[0] > 0) {
    echo "<p>その名前(".$_SESSION['join']['name'].")はすでに使用されています。別名で登録してください。</p>";
} else {
    $encrypted_password = password_hash($_SESSION['join']['pass_word'], PASSWORD_DEFAULT);
    $sql = sprintf('INSERT INTO member SET name="%s", pass_word="%s", mail="%s"',
        sanitize($db, $_SESSION['join']['name']),
        sanitize($db, $encrypted_password),
        sanitize($db, $_SESSION['join']['mail'])
    );
    mysqli_query($db, $sql) or die(mysqli_error($db));
}
?>

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>ログイン情報チェック</title>
</head>
<p>以下の情報で登録されました。</p>
<ul>
    <li><b>Name: </b><?=$_SESSION['join']['name']?></li>
    <li><b>Password: パスワードは忘れないようにしてください！！</b></li>
    <li><b>Mail: </b><?=$_SESSION['join']['mail']?></li>
</ul>
<?php unset($_SESSION['join']) ?>
<a href="index.php">ログイン画面</a>
</html>
```





新規ユーザーを登録して，下記のように正常に登録でき，パスワードが表示されないことを確認しましょう。

[![img](14_update_system.assets/check_php_encrypted_output.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/check_php_encrypted_output.png)



実際に暗号化されたかどうか，memberテーブルを見て確認して下さい。pass_wordフィールドの値が下記のように解読不能な文字列になっていれば大丈夫です。

暗号化パスワードを保持するmemberテーブル

[![img](14_update_system.assets/member_table_encrypted.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/member_table_encrypted.png)



### index.php: パスワード入力と暗号化パスワードの確認

登録した新規ユーザーを暗号化したパスワードで認証してみます。暗号化パスワードと平文のパスワードの照合は`password_verify`関数を使って行います。

index.phpの変更箇所

[![img](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/index_php_encrypted.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/index_php_encrypted.png)



うまくログインできるようでしたら，既存ユーザーの情報は全てmemberテーブルから消去し，すべてのユーザーが暗号化パスワードになるように登録し直しましょう。
