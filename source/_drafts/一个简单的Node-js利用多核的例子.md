---
title: 一个简单的Node.js利用多核的例子
id: 663
categories:
  - 技术专栏
tags:
---

/**
*
* User: maskwang
* Date: 12-11-15
* Time: 下午3:47
*/

var cluster = require('cluster');
var http = require('http');
var numCpus = require('os').cpus().length;

if (cluster.isMaster) {
//Fork workers.
for (var i = 0; i &lt; numCpus; i++) {
cluster.fork();
console.log('fork:' + (i+1));
}

cluster.on('exit', function(worker, code, signal) {
console.log('worker ' + worker.process.pid + ' died');
});
} else {
//Workers can share any TCP connection
// In this case its a HTTP server
http.createServer(function(req, res) {
res.writeHead(200);
res.end('Hello World---Mask!\n');
}).listen(8080);
}