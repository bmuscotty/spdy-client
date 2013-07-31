?spdy-client

This fork adds the HTTP version negociation in clear (http://tools.ietf.org/html/draft-ietf-httpbis-http2-04#section-3.2) to SPDY client.

The intend of the work is to test upgrade interoperability during the interim meeting next week. 

With this addon the spdy-client module creates SPDY connexions which negociate the HTTP2 flavor (spdy/2, spdy/3, HTTP-DRAFT-04/2.0 ...) using HTTP1.1 upgrade field (like with WebSocket). 

Of course SPDY over TLS is still supported (I did not change anything on TLS path).

To have the SPDY server you have to use the patch of the node-spdy module : <fixme: add the https://github.com/indutny/node-spdy

 ?Status

This code is completely inmature and is expected to be modified every days.

 ?Usage

At this step I did not adapt the code for post, ping and push. So only the get is supported (partially tested !).


?Example

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

 