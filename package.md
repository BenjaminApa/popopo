# application packaging

1. write a working command like `docker run -v ...:/app.js -p ... node node /app.js`
1. create a dockerfile based on ubuntu
2. create a dockerfile based on alpine linux
3. compare size
4. tag image and upload to your docker-hub or quay.io account
5. inspect docker image
6. upload docker image

# sample app

```javascript

```javascript
// curl https://localhost:8080/
require("http").createServer((req, res) => res.end(`hello [${req.url}] (${new Date()})\n`)).listen(8080)
```

# sample app with https

```javascript
// curl -k https://localhost:8443/
const https = require("https");
const fs = require("fs");
const handler = (req, res) => res.end(`hello [${req.url}] (${new Date()})\n`)
https.createServer({ 
    key: fs.readFileSync("/.../key.pem"), 
    cert: fs.readFileSync("/.../cert.pem") 
}, handler).listen(8443)
```
