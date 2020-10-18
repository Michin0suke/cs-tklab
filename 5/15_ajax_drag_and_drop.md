# AjaxとファイルのDrag & Drop処理

------

## JavaScriptのファイルAPI

ファイルAPI([File API](https://www.w3.org/TR/FileAPI/))とは，ブラウザ側にあるファイルを扱うためのJavaScript(ECMAScript)のAPI規格です。現行出回っているGoogle Chrome, Firefox, Internet Explorer, Microsoft Edgeで使用でき，ファイルの読み書きをJavaScriptから行うことができるようになります。

以下のHTML+JavaScriptでファイルAPIの機能を確認しましょう。

file_api.html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>File API confirmation</title>
    <style>
        #drop { border: solid 1px; }
    </style>
</head>
<body>
    <h1>ファイルAPI</h1>
    <div id="drop" ondragover="onDragOver(event)" ondrop="onDrop(event)">
        ここが<br>
        Dropエリア
    </div>
    <p>以下にファイル属性を表示</p>
    <p id="disp"></p>
    <p><a href="./">戻る</a></p>

    <script>
        if(File){
            // File APIに関する処理をここに書く
            alert('File API使用可能！');
        } else {
            alert('このブラウザではFile APIが使えません。');
        }
        var drop = document.getElementById('drop');
        var disp = document.getElementById('disp');
        function onDragOver(e) {
            event.preventDefault();
        }
        function onDrop(e) {
            var files = e.dataTransfer.files;
            disp.innerHTML = '';
            for(var i = 0; i < files.length; i++) {
                disp.innerHTML += `ファイル名: ${files[i].name}<br>`
            }
            event.preventDefault();
        }
    </script>
</body>
</html>
```



これを実行すると，ファイルAPIが実行可能であれば



[![img](15_ajax_drag_and_drop.assets/file_api_html_ok.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/file_api_html_ok.png)



という確認ウィンドウが出て，OKボタンを押すと下記の画面に移行します。



[![img](15_ajax_drag_and_drop.assets/file_api_html_first.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/file_api_html_first.png)





file_api.htmlの38行目～41行目に当たる`id=drop`のdivタグ部分に適当なファイルをDrag&Dropすると，22行目～33行目のDrop時に実行される`onDrop`関数が働き，下記のように`id=disp`のdivタグにファイル名が表示されるはずです。確認してみて下さい。



[![img](15_ajax_drag_and_drop.assets/file_api_html_second.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/file_api_html_second.png)



## XMLHttpRequest関数を用いたAjax非同期通信

Ajax(Asyncronus JavaScript + XML)とは，非同期のHTTP通信を可能とする技法で，JavaScriptの`XMLHttpRequest`関数を利用するのが標準のAjaxです。

下記はファイルのアップロードをDrag & Dropだけで実行するためのHTML+JavaScript, PHPスクリプトです。

file_api_upload.html



[![img](15_ajax_drag_and_drop.assets/file_api_upload_html.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/file_api_upload_html.png)



upload.php

[![img](15_ajax_drag_and_drop.assets/upload_php.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/upload_php.png)



これを実行すると下記のようにDrag & Dropのためのエリアが表示されます。



[![img](15_ajax_drag_and_drop.assets/file_api_upload_html_view_before.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/file_api_upload_html_view_before.png)



このエリアに画像ファイルを2つDrag & Dropし，「ファイル送信！」ボタンをクリックするとアップロードが開始され，無事終了すると下記のような表示を得ることができます。



[![img](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/file_api_upload_html_view.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/file_api_upload_html_view.png)



今度は，「ファイル送信！」ボタンを使わず，Drag & Dropした直後に自動的にアップロードが開始されるようにJavaScriptを改良します。

file_api_quick_upload.html

[![img](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/file_api_quick_upload_html.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/file_api_quick_upload_html.png)



送信ボタンが不要になりますので，下記のようにDrag & Dropエリアのみになります。



[![img](15_ajax_drag_and_drop.assets/file_api_quick_upload_html_view_before.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/file_api_quick_upload_html_view_before.png)



前述の例と同様，このエリアに画像ファイルを2つDrag & Dropした結果，アップロード処理完了後，下記のような画面を即座に見ることができます。



[![img](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/file_api_quick_upload_html_view.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/file_api_quick_upload_html_view.png)





## jQueryを用いたAjax非同期通信

上記のXMLHttpRequest関数を使った例を，jQueryのAjax機能(`$.ajax関数`)を利用して書き換えたものが下記になります。

ファイルアップロード処理を行うPHPスクリプトを変更する必要はありません。

file_api_upload_jquery.html

[![img](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/file_api_upload_jquery_html.png)](http://cs-tklab.na-inet.jp/phpdb/Chapter5/fig/file_api_upload_jquery_html.png)


