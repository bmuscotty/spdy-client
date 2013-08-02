spdy_client_http_upgrade:
------------------------

This fork of https://github.com/nasss/spdy-client adds the HTTP version negociation in clear (http://tools.ietf.org/html/draft-ietf-httpbis-http2-04#section-3.2) to SPDY client.

The intend of the work is to make some 'HTTP2 upgrade in the clear' interoperability tests during the HTTP2 interim meeting next week in Hambourg. 

With this addon the spdy-client is able to negociate the HTTP2 flavor (spdy/2, spdy/3, HTTP-DRAFT-04/2.0 ...) using HTTP1.1 Upgrade field, See the Data directory for tcpdump capture.


Notes:
-----
The node-spdy server which supports HTTP version negociation in clear is available at https://github.com/oln-es/spdy-server-http-upgrade. 


Examples:
--------

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