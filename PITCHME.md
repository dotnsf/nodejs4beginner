# はじめての Node.js 



+++

## Node.js

### サーバーサイド JavaScript

- シングルスレッド　＆　非同期処理

- npm(node package manager) によるパッケージ管理

    - [npm](https://www.npmjs.com/)

    - 便利な非標準機能を簡単に探してインストールする仕組み

        - HTTPサーバー、認証、圧縮／展開、画像処理、ファイルアップロード、テンプレート、・・

### 特徴

- 非同期実行による高速（に見える）処理

    - ディスク読み書きなどの時間のかかる処理では終了を待たずに次の処理を行うことができる

    - （待ち時間を最小化することができるので）CPU を効率的に利用できる

- 非同期実行の制御が特徴的

    - イベントの終了タイミングを意識する必要がある

+++

## インストール

### [メインページ](https://nodejs.org/) からダウンロードなど

- Windows : 上記ページよりバイナリをダウンロード＆インストール

- RedHat/CentOS : epel リポジトリから nodejs と npm をインストール

    - (epel) $ sudo yum install epel-release

    - (nodejs, npm) $ sudo yum install nodejs npm

- Ubuntu/Raspbian : （普通に）nodejs と npm をインストール

    - (nodejs, npm) $ sudo apt-get install nodejs npm

- Mac OS X : nodebrew からインストール

    - (homebrew) $ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

    - (nodebrew) $ brew install nodebrew

    - (nodejs, npm) $ nodebrew install-binary latest

+++

## ハローワールド

### 以下の内容を hello.js というファイル名で作成

- $ node hello

    - 「ハローワールド」と出力されれば成功

```hello.js
// hello.js
console.log( 'ハローワールド' );
```

+++

## 1 から 100 まで合算

### 以下の内容を sum.js というファイル名で作成

- $ node sum

    - 5050 と出力されれば成功

```sum.js
// sum.js
var n = 100;
var sum = 0;
for( var i = 1; i <= n; i ++ ){
  sum += i;
}
console.log( 'sum = ' + sum );
```

+++

## 1000 以下の素数

### 以下の内容を primenum.js というファイル名で作成

- $ node primenum

```primenum.js
// primenum.js
var n = 1000;
for( var i = 2; i <= n; i ++ ){
  if( isPrimeNum( i ) ){
    console.log( i );
  }
}

function isPrimeNum( x ){
  var b = true;
  var m = Math.floor( Math.sqrt( x ) );
  for( var j = 2; j <= m && b; j ++ ){
    b = ( x % j ) > 0;
  }

  return b;
}
```

+++

## Express を使ったウェブアプリケーション

### 以下の内容を app.js というファイル名で作成

- $ npm install express

- $ node app

    - ウェブブラウザで http://localhost:3000/hello にアクセスし、'Hello. It works!' と表示されれば成功

```app.js
// app.js
var express = require( 'express' );
var app = express();

app.get( '/hello', function( req, res ){
  res.write( 'Hello. It works!' );
  res.end();
});

var port = 3000;
app.listen( port );
console.log( 'server started on ' + port + ' ...' );
```

+++

## スタティックコンテンツ対応

- $ mkdir public

### 以下の内容を app.js , public/index.html というファイル名で作成

- $ node app

- ウェブブラウザで http://localhost:3000/ にアクセスし、入力フォームが表示されれば成功

```app.js
// app.js
var express = require( 'express' );
var app = express();

app.use( express.static( __dirname + '/public' ) );

app.get( '/hello', function( req, res ){
  res.write( 'Hello. It works!' );
  res.end();
});

var port = 3000;
app.listen( port );
console.log( 'server started on ' + port + ' ...' );
```

```public/index.html
<!-- public/index.html -->
<html>
<head>
<title>Hello</title>
</head>
<body>
<h1>Hello.</h1>
<hr/>
<form action="/show" method="post">
<input type="text" name="text" value="abc123"/>
<input type="submit" value="送信"/>
</form>
</body>
</html>
```

+++

## ポストデータを受け取る


+++



+++



+++




