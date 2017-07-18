# はじめての Node.js 

+++

## ハローワールド

+++

```hello.js
// hello.js
console.log( 'ハローワールド' );
```

+++

## 1 から 100 までの合算

+++

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

+++

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

+++

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

+++

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

+++

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

```app.js
// app.js
var express = require( 'express' );
var bodyParser = require( 'body-parser' );
var app = express();

app.use( bodyParser.urlencoded( { extended: true } ) );
app.use( bodyParser.json() );
app.use( express.static( __dirname + '/public' ) );

app.get( '/hello', function( req, res ){
  res.write( 'Hello. It works!' );
  res.end();
});

app.post( '/show', function( req,res ){
  var text = req.body.text;
  res.write( 'You wrote: ' + text );
  res.end();
});

var port = 3000;
app.listen( port );
console.log( 'server started on ' + port + ' ...' );
```

+++

## JSON(JavaScript Object Notation) オブジェクト

+++

```jsonobject.js
// jsonobject.js
var obj = {
  name1: 1,
  name2: 'b',
  name3: { key1: 'name3-key1' },
  name4: [ 1, 2, 3.45, 'abc', true ]
};

console.log( obj.name1 );
console.log( obj['name2'] );
console.log( obj.name3.key1 );
console.log( obj.name4.length );
for( var i = 0; i < obj.name4.length; i ++ ){
  console.log( obj.name4[i] );
}
```

+++

## ファイル読み込み（１）

+++

```readfile.js
// readfile.js
var fs = require( 'fs' );

fs.readFile( './aaa.xml', function( err, data ){
  if( err ){
    console.log( 'Error: ' + err );
  }else{
    console.log( data.toString() );
  }
});
console.log( 'end' );
```

+++

## ファイル読み込み（２）

+++

```aaa.xml
<?xml version="1.0"?>
<score-partwise>
 <work>
  <work-title>Do-re-mi</work-title>
 </work>
 <part-lit>
  <score-part id="P1">
   <part-name>Piano</part-name>
  </score-part>
 </part-lit>
</score-partwise>
```

+++

## ファイル読み込み（３）

+++

```readfile.js
// readfile.js
var fs = require( 'fs' );
var xml2js = require( 'xml2js' );

fs.readFile( './aaa.xml', function( err, data ){
  if( err ){
    console.log( 'Error: ' + err );
  }else{
    xml2js.parseString( data.toString(), function( err, result ){
      if( err ){
        console.log( 'Error: ' + err );
      }else{
        console.log( JSON.stringify( result, 2, null ) );
        //console.log( result['score-partwise']['work'][0]['work-title'][0] );
      }
    });
  }
});
console.log( 'end' );
```

+++

## ファイルアップロード

+++

```public/upload.html
<!-- public/upload.html -->
<html>
<head>
<title>Upload</title>
</head>
<body>
<h1>Upload</h1>
<hr/>
<form action="/upload" method="post" enctype="multipart/form-data">
<input type="file" name="music"/>
<input type="submit" value="送信"/>
</form>
</body>
</html>
```

+++

```app.js
// app.js
var express = require( 'express' );
var bodyParser = require( 'body-parser' );
var fs = require( 'fs' );
var xml2js = require( 'xml2js' );
var multer = require( 'multer' );
var app = express();

app.use( multer( { dest: './tmp/' } ).single( 'music' ) );
app.use( bodyParser.urlencoded( { extended: true } ) );
app.use( bodyParser.json() );
app.use( express.static( __dirname + '/public' ) );

app.get( '/hello', function( req, res ){
  res.write( 'Hello. It works!' );
  res.end();
});

app.post( '/show', function( req,res ){
  var text = req.body.text;
  res.write( 'You wrote: ' + text );
  res.end();
});

app.post( '/upload', function( req,res ){
  var originalname = req.file.originalname;
  var path = req.file.path;
  if( originalname.endsWith( '.xml' ) ){
    fs.readFile( path, function( err, data ){
      xml2js.parseString( data.toString(), function( err, result ){
        res.write( result['score-partwise']['work'][0]['work-title'][0] );
        res.end();
      });
      fs.unlink( path, function( err ){} );
    });
  }else{
    fs.unlink( path, function( err ){} );
    res.write( 'Error: not XML file.' );
    res.end();
  }
});

var port = 3000;
app.listen( port );
console.log( 'server started on ' + port + ' ...' );
```

+++

## ポート番号自動化

+++

```app.js
// app.js
var express = require( 'express' );
var cfenv = require( 'cfenv' );
var app = express();

app.get( '/hello', function( req, res ){
  res.write( 'Hello. It works!' );
  res.end();
});

var appEnv = cfenv.getAppEnv();
var port = appEnv.port || 3000;
app.listen( port );
console.log( 'server started on ' + port + ' ...' );
```

+++

## テンプレート

+++

```app.js
// app.js
var express = require( 'express' );
var cfenv = require( 'cfenv' );
var ejs = require( 'ejs' );
var fs = require( 'fs' );
var app = express();

app.get( '/hello', function( req, res ){
  res.write( 'Hello. It works!' );
  res.end();
});

app.get( '/list', function( req, res ){
  var template = fs.readFileSync( __dirname + '/list.ejs', 'utf-8' );
  var p = ejs.render( template, { title: 'EJS', body: '<h1>Hello, EJS</h1>' );
  res.write( p );
  res.end();
});

var appEnv = cfenv.getAppEnv();
var port = appEnv.port || 3000;
app.listen( port );
console.log( 'server started on ' + port + ' ...' );
```

+++

```list.ejs
<!-- list.ejs -->
<html>
<head>
<title><%= title %></title>
</head>
<body>
<%- body %>
</body>
</html>
```

+++

## テンプレートに配列を渡す

+++

```app.js
// app.js
var express = require( 'express' );
var cfenv = require( 'cfenv' );
var ejs = require( 'ejs' );
var fs = require( 'fs' );
var app = express();

app.get( '/hello', function( req, res ){
  res.write( 'Hello. It works!' );
  res.end();
});

app.get( '/list', function( req, res ){
  var template = fs.readFileSync( __dirname + '/list.ejs', 'utf-8' );
  var values = [ 1, 3, 4, 10, 2, -5 ];
  var p = ejs.render( template, { title: 'EJS', values: values );
  res.write( p );
  res.end();
});

var appEnv = cfenv.getAppEnv();
var port = appEnv.port || 3000;
app.listen( port );
console.log( 'server started on ' + port + ' ...' );
```

+++

```list.ejs
<!-- list.ejs -->
<html>
<head>
<title><%= title %></title>
</head>
<body>
<ul>
<% for( var i = 0; i < values.length; i ++ ){ %>
<li><%= values[i] %></li>
<% } %>
</ul>
</body>
</html>
```

+++
