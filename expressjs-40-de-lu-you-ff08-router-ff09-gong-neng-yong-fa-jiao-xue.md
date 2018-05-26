# Express.js 4.0 的路由（Router）功能用法教學

Express.js 4.0 有加入一個新的 Router 功能，它就像一個迷你的應用程式，可以讓應用程式內部的路由撰寫更方便、更有彈性。

Express.js 在 4.0 版中有許多新的功能，其中一項主要的功能就是 Router，以下我們介紹如何使用 Router 功能來撰寫應用程式。

## 基本應用程式

首先建立一個`package.json`檔案，定義套件的相依資訊：

```
{
    "name": "express-router-experiments",
    "main": "server.js",
    "dependencies": {
        "express": "~4.0.0"
    }
}
```

建立好`package.json`之後，就可以使用`npm`指令自動安裝所需要的套件：

```
npm install
```

接著建立主要的`server.js`，其內容如下：

```
// ---- 基本設定 ----
var express = require('express');
var app     = express();
var port    = process.env.PORT || 8080;

// ---- ROUTES ----

// 舊方法
app.get('/sample', function(req, res) {
  res.send('this is a sample!');
});

// ---- 啟動伺服器 ----
app.listen(port);
```

接著就可以測試一下伺服器是否可以正常運作：

```
node server.js
```

正常來說，這時候開啟瀏覽器輸入`http://localhost:8080/sample/`這個網址，就可以看到

> this is a sample!

這樣的訊息。

這個範例中，我們使用`app.get`來處理路由的問題，這種方式是 Express 3.0 的用法，接下來我們會使用 Express 4.0 的 Router 功能來加入更多的路由。

## Express Router

我們在既有的路由之後，使用新的 Router 功能加上額外的一些路由設定：

```
// ---- 基本設定 ----
var express = require('express');
var app     = express();
var port    = process.env.PORT || 8080;

// ---- ROUTES ----

// 舊方法
app.get('/sample', function(req, res) {
  res.send('this is a sample!');
});

// Express Router

// 建立 Router 物件
var router = express.Router();

// 首頁路由 (http://localhost:8080)
router.get('/', function(req, res) {
  res.send('home page!');
});

// 另一張網頁路由 (http://localhost:8080/about)
router.get('/about', function(req, res) {
  res.send('about page!');
});

// 將路由套用至應用程式
app.use('/', router);

// ---- 啟動伺服器 ----
app.listen(port);
```

這裡我們建立一個`Router`物件，然後設定這個物件的路由規則，最後再將這個`Router`物件的路由規則套用至應用程式中：

```
app.use('/', router);
```

這樣一來，我們就建立了`http://localhost:8080`與`http://localhost:8080/about`這兩張網頁。

將路由套用至應用程式時，可以指定路由的基礎路徑，舉例來說，如果我們將路徑指定為`/app`

```
app.use('/app', router);
```

這樣建立的兩個路由就會變成`http://localhost:8080/app`與`http://localhost:8080/app/about`。

這樣的特性可以讓我們很方便地將不同功能的路由區分開來，分別建立不同的`Router`物件，以不同的路徑套用至應用程式中，讓程式結構模組化且更有彈性。

## Route Middleware

Express 的 middleware 功能可以讓請求（request）在被處理之前，先執行一些前置作業，例如檢查使用者是否有登入或是紀錄一些使用者的瀏覽資料等等，凡是需要在實際處理請求之前要做的動作，都可以使用 middleware 來處理。

下面這個範例是一個簡單的 middleware，它會在每一個請求被處理之前，輸出一行紀錄訊息到終端機上：

```
// ... (略)

// 建立 Router 物件
var router = express.Router();

// 在每一個請求被處理之前都會執行的 middleware
router.use(function(req, res, next) {

  // 輸出記錄訊息至終端機
  console.log(req.method, req.url);

  // 繼續路由處理
  next();
});

// 首頁路由 (http://localhost:8080)
router.get('/', function(req, res) {
  res.send('home page!');
});

// 另一張網頁路由 (http://localhost:8080/about)
router.get('/about', function(req, res) {
  res.send('about page!');
});

// 將路由套用至應用程式
app.use('/', router);

// ... (略)
```

這樣一來，只要有任何請求要處理時，終端機上就會輸出對應的紀錄訊息。

在使用 middleware 時必須要注意他的放置位置必須要在 routes 之前，程式在執行的時候會依據 middleware 與 routes 的先後順序來執行，如果不小心將 middleware 放在 routes 之後，那麼在 routes 處理完請求之後就會結束處理的流程，這樣 middleware 就根本不會執行。

## 參數路由（Route with Parameters）

路由的規則除了使用固定的字串之外，也可以包含會變動參數，下面這個例子可以將使用者的名稱透過 URL 傳入程式中，並且根據使用者的名稱輸出訊息：

```
// ... (略)

// 含有參數的路由 (http://localhost:8080/hello/:name)
router.get('/hello/:name', function(req, res) {
  res.send('hello ' + req.params.name + '!');
});

// 將路由套用至應用程式
app.use('/', router);

// ... (略)
```

這裡的`:name`就像一個變數名稱，如果我們輸入的 URL 為`http://localhost:8080/hello/seal`，那麼在程式中的

`req.params.name`所抓取到的值就會是`'seal'`，透過這樣的機制我們就可以跟不同的使用者打招呼。

### 驗證參數

有時候我們會需要針對傳入的路由參數來進行篩選或驗證，例如檢查使用者所輸入的字串是否是合法的名稱，這時候就可以使用`.param()`這個專門用來處理參數的 middleware：

```
// ... (略)

// 驗證 :name 的 route middleware
router.param('name', function(req, res, next, name) {

  // 在這裡驗證資料
  // ... ... ...

  // 顯示驗證訊息
  console.log('doing name validations on ' + name);

  // 當驗證成功時，將其儲存至 req
  req.name = name;

  // 繼續後續的處理流程
  next();
});

// 含有參數的路由 (http://localhost:8080/hello/:name)
router.get('/hello/:name', function(req, res) {
  res.send('hello ' + req.name + '!');
});

// 將路由套用至應用程式
app.use('/', router);

// ... (略)
```

這樣在每次有`:name`參數傳入時，就會先執行這裡新增的 middleware，經過驗證確定沒問題之後，再將傳入的名稱儲存至`req`中，透過這樣的方式將驗證過的資料傳遞給`.get`路由，所以在`.get`路由中，我們也將原本的`req.params.name`改為`req.name`。

現在當我們開啟`http://localhost:8080/hello/seal`的時候，終端機中就會出現驗證的訊息：

## 登入路由

除了使用`express.Router()`的方式來建立路由之外，我們也可以使用`app.route`直接在應用程式上新增路由，這種方式是 Router 的簡略寫法，語法看起來就跟傳統上的`app.get`很類似。

使用`app.route`有個好處是可以讓我們一次針對一個路由新增好幾個動作，例如我們想要使用`GET`顯示登入表單，並且使用`POST`處理表單：

```
// ... (略)

// ---- ROUTES ----

app.route('/login')

  // 顯示登入表單 (GET http://localhost:8080/login)
  .get(function(req, res) {
    res.send('this is the login form');
  })

  // 處理登入表單 (POST http://localhost:8080/login)
  .post(function(req, res) {
    console.log('processing');
    res.send('processing the login form!');
  });

// ... (略)
```

這樣一來程式就可以在`/login`這個路由上將`GET`與`POST`分開處理，而且這樣的寫法既方便又簡潔。

