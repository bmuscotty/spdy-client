This fork adds the HTTP version negociation in clear (http://tools.ietf.org/html/draft-ietf-httpbis-http2-04#section-3.2) to SPDY client.

The intend of the work is to test 'HTTP2 upgrade in the clear' interoperability during the interim meeting next week. 

With this addon the spdy-client module creates SPDY connexions which negociate the HTTP2 flavor (spdy/2, spdy/3, HTTP-DRAFT-04/2.0 ...) using HTTP1.1 Upgrade field (like with WebSocket). 

Of course SPDY over TLS is still supported (I did not change anything on TLS path).


The SPDY server you have to use the patch of the 
node-spdy module : <fixme: add the https://github.com/indutny/node-spdy


Examples:


GET request example for SPDY:
```javascript

 
var req = client.get(
    {
	path : '/flags/world-flags.htm'
	,url : '/'
	,port: 1337
	,host: 'localhost'
	,plain : true // USE plain for SPDY over tcp connection, TLS otherwise
	,version: 3  
    },
    function(response){
	    logger.info("--- GET  RESPONSE --");
	    response.once('data', function (chunk) {
		    var data = String.fromCharCode.apply(null, new Uint16Array(chunk));
		    logger.info(data);          
	});    

});
```

GET request example for HTTP-DRAFT-04/2.0::

```javascript
var req = client.get(
    {
	path : '/flags/world-flags.htm'
	,url : '/'
	,port: 1337
	,host: 'localhost'
	,plain : true // USE plain tcp connection, TLS otherwise
	,version: 'HTTP-DRAFT-04/2.0' 
    },
    function(response){
	    logger.info("--- GET  RESPONSE --");
	    response.once('data', function (chunk) {
		    var data = String.fromCharCode.apply(null, new Uint16Array(chunk));
		    logger.info(data);          
	});    

}); 
```