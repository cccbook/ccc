# 請用 Node.js 設計一個留言板

以下是一個使用 Node.js 設計的留言板程式碼示例:

```js
const http = require('http');
const fs = require('fs');
const url = require('url');
const querystring = require('querystring');

const server = http.createServer((req, res) => {
  const path = url.parse(req.url).pathname;

  // 如果客戶端發出的請求是 /message，則處理留言請求
  if (path === '/message') {
    const query = querystring.parse(url.parse(req.url).query);

    // 獲取留言內容
    const message = query.message;

    // 將留言寫入 messages.txt 文件
    fs.appendFile('messages.txt', message + '\n', (err) => {
      if (err) {
        console.error(err);
      }
    });

    // 將留言內容顯示在頁面上
    res.end(message);
  } else {
    // 如果客戶端發出的請求是其它路徑，則載入 index.html 頁面
    fs.readFile('index.html', (err, data) => {
      if (err) {
        console.error(err);
        res.statusCode = 500;
        res.end('Internal Server Error');
      } else {
        res.end(data);
      }
    });
  }
});

server.listen(3000);
```

這段程式碼的作用是建立一個 HTTP 伺服器，當客戶端發出請求時，服務器會檢查客戶端請求的路徑，如果請求的路徑是 /message，則服務器會處理留言請求，將留言內容寫入 messages.txt 文件並顯示在頁面上；如果客戶端發出的請求是其它路徑，則服務器會載入 index.html 頁面。

除了上面的代碼之外，留言板程式還需要有一個 index.html 頁面來顯示留言板的界面，並且還需要通過 JavaScript 程式碼來處理客戶端的請求。具體的實現方式可以根據具體的需求進行擴展。以下是一個簡單的 index.html 頁面示例:



```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>留言板</title>
</head>
<body>
  <h1>留言板</h1>
  <form>
    <textarea name="message"></textarea>
    <br>
    <input type="submit" value="提交留言">
  </form>
</body>
</html>

```

這段頁面代碼會顯示一個簡單的留言板界面，包括一個留言輸入框和一個提交按鈕。

下面是一個JavaScript 程式碼示例，用於處理客戶端的留言請求:

```
const form = document.querySelector('form');

form.addEventListener('submit', (event) => {
  event.preventDefault();

  // 獲取留言內容
  const message = form.elements.message.value;

  // 將留言發送到服務器
  fetch('/message?message=' + encodeURIComponent(message))
    .then((res) => res.text())
    .then((message) => {
      // 將留言顯示在頁面上
      const p = document.createElement('p');
      p.textContent = message;
      document.body.appendChild(p);
    });
});
```

這段代碼會在留言表單被提交時觸發，獲取留言內容並通過 fetch 函數發送到服務器，然後在服務器返回留言內容後將其顯示在頁面上。

綜上所述，我們可以使用 Node.js 設計一個簡單的留言板程式，通過服務器和客戶端的交互來實現留言的處理與顯示。

```
