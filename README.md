# DK
DK is named after  donkey_proxy,which can branch whole requests into diff cases ,each case is hold by diff company's department .
some feature :Group,ABtest,Filter.
[![Build
Status](https://travis-ci.org/linsomniac/python-memcached.svg)](https://github.com/facebookarchive/scribe)

## Features
* Group
  * http_header
 		
 		http request header contains url(host,path,querystring)，some dynamic programer can group it into branches .
  * http_status
  
  	  http_status==200 is ok ,otherwise ,the status_40x and status_50x are very usefull for company's SEO, analysis by httpstaus is a great tool in KPI
  	  
* Filter
	* anti_spider  	  
 		
 		antispider is a sub http request forked by main request before finish response
 		```
 		if grouped 
 			fork a sub_request and wait or timeout 
 			wait and recv status ,deny or pass by return_status
 		
 		endif 
    then tracke user by cookie ,which prove other requests from this user  is good . 	
 		```	
   * XSS+CRLF
   		* httpmethod==GET (path+querystring)
   		  ```
        str.replace("\r\n")||str.replace(url_encode("\r\n"))
        ``` 
		  ```
        XssPattern='/>\w*[^>]*?[\"|\']>\w*[^>]*?/i';
				if reg_match 
					foreach (querstring :item) 
							earse (xss)
					build new querystring					upstream
			```
	* cookie
	
		cookie is defined machine automatic ,mustn't be defined by develper , some dev-cookie is store in redis .
		```
		cookie_session:get(key),set(key,v)
		cookie_origin:get(key),set(key,v) 
		cookie_raw:get(key),set(key,v) 
		```
		QPS:10w (50+ redis slave cluster,consistent hash key )   		
* ABtest	
	* not average  		
		* * divid by uuid (user uniq id )
		* * divid by random 	
		*  divid by user_define (like ip, some stable factors  )
	* average 
		* * divid by server_session_id(inc by server ) 	 
	all ABtest callback is based cookie , on the other handle ,if cookie is cleared suddenly ,ABtest disappear  	
	 
## Overview
DK is a shorted name for donkey_proxy, which is scripted by  nginx +lua ,DK is aimed at branching mycompany's HTTP requests into diff cluster . while mime request look like so much common ,but few diff in urlPatter. 

This software is a 100% Lua interface to the IO or  memory cache
daemon.  It is the HTTPPROXY side software which listening 80 +443 port 
in one or more, possibly redis ,memcached . for more information please contact us  

Please report issues and submit code changes to the github repository at:

	https://github.com/gjpicker/dk.git

## Depend
```
tengine
luajit
luarocks
luarocks install lrexlib-PCRE
luarocks install luasocket
luarocks install lua-cjson
lua-resty-memcached
```
## Testing

Test patches locally and easily by running lua cli:

    cd nginx/lua/data
    lua check.lua
