# express-statsd-route (原始项目：[Uber Express statsd](https://github.com/uber/express-statsd))
> Telegraf cheniu_api 项目埋点方案，采用 express 的 req.route 对象作为API埋点 key。

## Installation
``` bash
npm install express-statsd-route
```

## Usage
A Example Of Express，File was located in `test/app.js`
```node
'use strict';
var express = require('express');
var expressStatsdRoute = require('../');
var app = express();


const DEBUG = require('debug')('Test:statsd');
const PORT = 8080;


app.use(expressStatsdRoute({
    keyPrefix: 'ESRTEST.',
    keyGeneFunc: function(req){
        return req.route && req.route.path.split('/').join('.').substr(1);
    }
}));

app.get('/hello_world/:name', function(req, res){
    res.send('hello world, ' +  req.params['name']);
});

app.get('/your_name', function(req,res){
    res.send('my name is ESR');
});



app.listen(PORT, function(){
    DEBUG('server listen on %d', PORT);
});
```

**NOTICE**  
最终组合的 key 需以 `.` 相连，如 part1.part2.part3 


## Options
```
expressStatsdRoute(options);
```
* host: [optional] statsd server host, default 127.0.0.1
* port: [optional] statsd server port, default 8125
* keyPrefix: [required] A code represents your project
* keyGeneFunc: [required] A function to generate your api key
