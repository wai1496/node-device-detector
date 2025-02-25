Use in the express.js server
-
```js
const express = require('express');
const DeviceDetector = require('node-device-detector');


// init app
const deviceDetector = new DeviceDetector;
const app = express();
const port = 3001;


const hasBotResult = (result) => {
  return result && result.name;
}

// create middleware
const middlewareDetect = (req, res, next) => {
  const useragent = req.headers['user-agent']; 
  req.useragent = useragent;
  req.device = deviceDetector.detect(useragent);
  req.bot = deviceDetector.parseBot(useragent);
  next();
};

// attach middleware
app.use(middlewareDetect);

// create test route
app.get('/detect', (req, res) => {
   let useragent = req.useragent;
   let detectResult = req.device;
   let botResult = req.bot;
   res.send(JSON.stringify({useragent, detectResult, botResult, isBot: hasBotResult(botResult)}));
})

app.listen(port, () => {
  console.log('server listen port %s', port);
})
```