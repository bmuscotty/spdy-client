spdy-client
===========

This fork adds the HTTP version negociation in clear (http://tools.ietf.org/html/draft-ietf-httpbis-http2-04#section-3.2) to SPDY client.


With this addon the module, you can create SPDY connexions which negociate the HTTP2 flavor (spdy/2, spdy/3, HTTP-DRAFT-04/2.0 ...) using HTTP1.1 upgrade field (like with WebSocket). 

Of course SPDY over TLS is still supported (I have to test it to be sure but did not change anything on this path).

-------------------------------
You can send requests to the SPDY server and add listeners for response or data events.

You need node-spdy module : <fixme: add the https://github.com/indutny/node-spdy

Usage
===========


At this step I did not adapt the code for push, ping and push. So only the get is supported (tested)

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

 var req = client.get(
    {
	path : '/'
	,url : '/'
	,port: 1337
	,host: 'localhost'
	,plain : true // USE plain tcp connection, TLS otherwise
	,version: 'HTTP-DRAFT-04/2.0' //means 
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
 


