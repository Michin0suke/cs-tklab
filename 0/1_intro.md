# 本教材の目的

21世紀の現在，インターネット(The Internet)は，ビジネスから私生活に至るあらゆる目的で，通信可能なあらゆる場所で使われています。

そのためインターネット上でサービスを提供できるWebアプリケーションは必要不可欠なものとなっています。

この教材は，HTML，CSS，JavaScript，PHP，MySQL等を活用した動的Webページの作成方法を学び，複雑なWebアプリケーションを作成できるスキルを身につけることを最終目標としています。

## Webアプリケーションの概略

ここではWebアプリケーション(Web Application)，ネイティブ(native) アプリケーションとは何か，またその仕組みについて説明します。

端的には次のように分類されます。

* **Webアプリケーション**・・・WebブラウザとWebサーバのやり取りを通じて動作するアプリケーション（インストールを必要としない）
* **ネイティブアプリケーション**・・・端末にインストールして使用するアプリケーション



この二つのアプリケーションの違いをソフトウェア階層図として表現したものが下記になります。

[![img](http://cs-tklab.na-inet.jp/phpdb/Introduction/fig/Intro.PNG)](http://cs-tklab.na-inet.jp/phpdb/Introduction/fig/Intro.PNG)

上の図はWebアプリケーションとネイティブアプリケーションの仕組みの違いを表したものです。

ネイティブアプリケーションはハードウェア及びＯＳ上で動作するのに対し， WebアプリケーションはWebサーバ上に設置され，ブラウザからの要求を受けてサービスを提供します。 こちらはWebサーバソフトとブラウザをLANで結び，情報をやり取りすることで成り立っています。

## Webアプリケーションの長所・短所

Webアプリケーションはブラウザと(Web)サーバとの協調によって構築されたサービスなので，次のような長所と短所があります。

* Webアプリケーションの長所

  ― ブラウザとサーバでコンピュータ資源を分担しあえる

  ― ダウンロードやインストールをする必要がない

  ― ひとつのWebアプリでいろいろな端末（ＯＳ）に対応させることができる

* Webアプリケーションの短所

  ― ネイティブアプリケーションに比べて処理が遅い

  ― 端末の機能（例：スマートフォンのカメラなど）を活かしづらい

ネイティブアプリケーションは，単独のハードウェア上で動作するシンプルなソフトウェアなので，Webアプリケーションと比べると，次のような長所と短所があります。

* ネイティブアプリケーションの長所

  ― 処理が速い

  ― 端末の機能を活かしやすい

* ネイティブアプリケーションの短所

  ― 端末やＯＳによって別々に開発しなければならない場合がある

Webアプリケーションとネイティブアプリケーションには，以上のような特徴があり，作成するシステムの内容によって使い分ける必要があります。この教材ではWebアプリケーションについての勉強をしていきます。

------

## 主要なWebサーバソフトウェアとブラウザ

Web(World Wide Web)というThe Internet上のサービスが登場して以来，Webサーバとブラウザは，激しいシェア争いを経て，数種類のソフトウェアのみが生き残り，今日もバージョンアップをし続けています。ここでは，主要なWebサーバソフトウェアとブラウザについて簡単に紹介します。

### Webサーバ側で動作する主要ソフトウェア

Webサーバ側では，Webサーバに加え，スクリプト実行コア，RDBMS(Relational DataBase Management System)というソフトウェアが必要になります。

* Webサーバ・・・ブラウザからの接続要求に対してファイルやプログラムの実行結果を返すソフトウェア
  * [Apache HTTPD](https://httpd.apache.org/) (アパッチ)
  * [nginx](https://www.nginx.org/)（エンジンエックス)
  * [Microsoft IIS](https://www.iis.net/)（Internet Information Service)
* RDBMS・・・アクセスしてくるユーザの情報を集約して保存するためのデータベース
  * [MySQL ](https://www.mysql.com/)(マイエスキューエル)
  * [MariaDB](https://mariadb.org/)
  * [SQL Server](https://www.microsoft.com/ja-jp/sql-server/sql-server-2016)
  * [SQLite](https://sqlite.org/)(エスキューライト，インストール不要のデータベースエンジン)
* スクリプト実行コア(インタプリタ)・・・Webサーバ上で動作し，Webサーバからの要求に答え，必要に応じてRDBMSとのやり取りも行って結果の出力を行うスクリプトの実行を司るソフトウェア
  * [PHP](http://php.net/)
  * [.NET Core ](https://docs.microsoft.com/ja-jp/dotnet/core/)(ドットネット）

上記3種類の主要ソフトウェアを，既存のOSと組み合わせて，例えば，下記の図に示すようなWebサーバのソフトウェアスタック（階層）が構成できます。

[![img](http://cs-tklab.na-inet.jp/phpdb/Introduction/fig/lampstack_new_small.png)](http://cs-tklab.na-inet.jp/phpdb/Introduction/fig/lampstack_new.png)

近年は，スクリプト言語パッケージに，簡易的なWebサーバ機能を備えた

* [Node.js](https://nodejs.org/)・・・[Chrome V8レンダリングエンジン](https://github.com/v8/v8/wiki)を備えたJavaScript開発環境。組み込み機器を含むIoT(Internet of Things)分野での使用例が沢山ある。
* [Ruby on Rails](http://rubyonrails.org/)・・・スクリプト言語RubyにWebサーバ(Rack)を加えた開発環境
* [PHP + 簡易サーバ機能](http://php.net/manual/ja/features.commandline.webserver.php)

等の開発環境の利用も進んできました。この場合は，下記の図のように，それぞれのスクリプト言語環境下で動作するWebサーバが同梱されていることになります。



[![img](http://cs-tklab.na-inet.jp/phpdb/Introduction/fig/script_server.png)](http://cs-tklab.na-inet.jp/phpdb/Introduction/fig/script_server.png)

これ以外にも，Javaを中核としたWebアプリケーションも官公庁を中心に導入されていますが，この教材では扱いません。

### 主要Webブラウザとレンダリングエンジン

Webページそのものは，下記の3つの要素で構成されたテキストファイル(+画像・動画ファイル等)となります。

* [HTML](http://cs-tklab.na-inet.jp/phpdb/Chapter1/html.html) ・・・ Webページの「構造」を[DOM(Domain Object Model)](http://cs-tklab.na-inet.jp/phpdb/Chapter1/dom_jquery.html)というツリーの形で表現したもの。Webページの土台にあたる。
* [CSS](http://cs-tklab.na-inet.jp/phpdb/Chapter1/css.html) ・・・ WebページのDOM単位で，見栄え（デザイン）を規定するスタイルシート。HTMLに埋め込んで使用する。
* [JavaScript(ECMAScript)](http://cs-tklab.na-inet.jp/phpdb/Chapter1/javascript.html) ・・・ Webページに動的な要素を加えるためのスクリプト言語。HTMLにスクリプト(プログラム)を埋め込んで使用する。

HTMLに記述されたDOM構造，CSSで指定されたデザイン，そしてJavaScriptで規定された動的な動作を一手に引き受けて画面に表現するソフトウェアがWebブラウザ(Web browser)です。

現在，PCやスマートフォンで使われている主要な(Web)ブラウザは，[Chrome](https://www.google.co.jp/chrome/), [Firefox](https://www.mozilla.jp/), Internet Explorer(もしくは[Edge](https://www.microsoft.com/ja-jp/windows/microsoft-edge))です。iPhoneではSafariがデフォルトで導入されています。

[![img](http://cs-tklab.na-inet.jp/phpdb/Introduction/fig/mainstream_browsers_small.png)](http://cs-tklab.na-inet.jp/phpdb/Introduction/fig/mainstream_browsers.png)

上記は3つの主要ブラウザの表示画面ですが，どれも同じWebページを閲覧しているにもかかわらず，微妙に画面表示が異なっていることが分かります。ユーザーの設定でどうとでも変更はできますが，デフォルト状態でもこのような違いが出るのは，画面表示を司るレンダリングエンジンがそれぞれ異なっていることにも起因します。

[![img](http://cs-tklab.na-inet.jp/phpdb/Introduction/fig/rendering_engine_small.png)](http://cs-tklab.na-inet.jp/phpdb/Introduction/fig/rendering_engine.png)

Webアプリケーションを作る際には，ブラウザ間の画面表示の違い，サポートしているHTML, JavaScript, CSSの違いについても気にかけるようにして下さい。

------

## XAMPP for Windowsのインストール

本教材では，Webサーバ側で動作するWebアプリケーション作成に際しては，XAMPP for Windowsを利用したApache + PHP + MySQL(MariaDB)によるWebプログラミング開発環境を利用します。基本的にはバックアップのしやすいポータブル版を使っていきますので，必ず適当なドライブのトップフォルダに"xampp"をインストールして下さい。インストール方法は[こちらを参照して下さい](https://cs-tklab.na-inet.jp/?page_id=568)。

また，今後作成する全てのテキストファイルやスクリプトは，UTF-8(8bit単位のUnicode)を使用して記述します。UTF-8を使用できる最新のWeb開発用のエディタ（下記参照）を使用するようにして下さい。

* [Visual Studio Code](https://www.microsoft.com/ja-jp/dev/products/code-vs.aspx)
* [Sublime Text](https://www.sublimetext.com/)
* [Eclipse](https://eclipse.org/)
* [Atom](https://atom.io/)



## SPAとハイブリッドアプリケーションについて

ブラウザ側のみで動作が完結するWebアプリケーション，特に，シングルページアプリケーション(SPA, Single Page Application)を作成するにあたっては，UTF-8が使用できるテキストエディタと，動作確認用のブラウザがあれば，HTML+CSS+JavaScriptの開発は可能となりますので，最小限の環境としては十分です。

近年は，ブラウザのレンダリングエンジンを組み合わせたJavaScript開発環境，[Node.js](https://nodejs.org/)の利用が盛んになっており，SPAにこのNode.jsのレンダリングエンジンを組み合わせてAndroidやiOS用のネイティブアプリケーションの開発ができる，ハイブリッドアプリケーション手法が注目されています。その際には，エディタとブラウザに加えて，[Apache Cordova](https://cordova.apache.org/)のようなハイブリッドアプリケーション開発環境や，[Android Studio](https://developer.android.com/studio/)のようなスマートフォンアプリの開発環境が必要になります。Windows環境での環境構築方法については[こちら](https://cs-tklab.na-inet.jp/?page_id=1728)を参照してください。