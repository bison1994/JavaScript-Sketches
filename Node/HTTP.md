# HTTP & HTTPs

HTTP deals with stream handling and message parsing only


### 发送请求

```js
const https = require('https')

https.get('https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY', (res) => {
  let data = ''

  // A chunk of data has been recieved
  res.on('data', (chunk) => {
    data += chunk
  })

  // The whole response has been received
  res.on('end', () => {
    console.log(JSON.parse(data).explanation)
  })

}).on("error", (err) => {
  console.log("Error: " + err.message)
})

// another way
const req = https.request(options, callback)
```


### 服务器

```js
const http = require('http')
const url = require('url')

// create a server with simple router
http.createServer(function (req, res) {
    switch (url.parse(req.url, true).pathname) {
        case '/':
            res.write('Hello World!')
            break
        default: {
            console.log(req.url)
        }
    }
    
    res.end()
}).listen(8080)
```