spdy-client
===========

With this module, you can create SPDY clients in node.js. You can send requests to the SPDY server and add listeners for response or data events.

You need node-spdy module : https://github.com/indutny/node-spdy

Usage
===========

var client = require('spdy-client');
var log4js = require('log4js');

var logger = log4js.getLogger('CLIENT-TEST');
logger.setLevel('ALL'); // ALL:   , TRACE:   , DEBUG:   , INFO:  , WARN:   , ERROR:   , FATAL:   , OFF: 
client.setLogLevel('ALL');


var req = client.get(
    {
	path : '/flags/world-flags.htm'
	,url : '/'
	,port: 1337
	,host: 'localhost'
	,plain : true // USE plain tcp connection, TLS otherwise
	,version: 3
    },
    function(response){
	    logger.info("--- GET  RESPONSE --");
	    response.once('data', function (chunk) {
		    var data = String.fromCharCode.apply(null, new Uint16Array(chunk));
		    logger.info(data);          
	});    

}); 


req.on('error', function(err){
      logger.error(err);
 });    

 