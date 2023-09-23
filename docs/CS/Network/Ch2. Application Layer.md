Delay


吞吐量：Throughput
bit/sec
实际吞吐量：Good put


>如果你对HTTP/1.1协议做过抓包分析，就会发现它是用“whitespace-delimited”方式编码的。用空格、回车来编码，是因为HTTP在诞生之初追求可读性，这样更有利于它的推广。
>
>然而在当下，这种低效的编码方式已经严重影响性能了，所以2009年Google推出了基于二进制的SPDY协议，大幅提升了编码效率。2015年，稍做改进后它被确定为HTTP/2协议，现在50%以上的站点都在使用它。

HTTP1.1是传字符串，后面已经优化了

Over the top OTT
>有点像 互联网电视 商业上：最早是篮球的用词，意思是把球从对手的头顶传过去，用打篮球的常用语，就是有人来防守，你把他晃过去了。我们看电视行业，大家会觉得这是一个封闭的圈子，广电管内容，运营商管线路，就他们俩一起控制了用户对内容的需求，其他人无法参与。而OTT的概念，是基于互联网这个大网，大家都挂在上面，当然就可以在这张网上开展很多工作，如通讯方面的skype ，现在的腾讯的微信。

#### video streaming
DASH Dynamic, Adaptive Streaming over HTTP
Streaming video = encoding + DASH + playout buffering


### HTTP 状态码

###### 1XX Information

    * 100 Continue
    * 101 Switching Protocols
    * 102 Processing
  
###### 2XX Success

     * 200 OK
     * 201 Created
     * 202 Accepted
     * 203 Non-Authoritative Information
     * 204 No Content
     * 205 Reset Content
     * 206 Partial Content
     * 207 Multi-Status
     * 208 Already Reported
     * 226 IM Used

###### 3XX Redirect

    * 300 Multiple Choices
    * 301 Moved Permanently
    * 302 Found
    * 303 See Other
    * 304 Not Modified
    * 305 Use Proxy
    * 306 Switch Proxy
    * 307 Temporary Redirect
    * 308 Permanent Redirect

###### 4XX Client Fail

    * 400 Bad Request
    * 401 Unauthorized
    * 403 Forbidden
    * 404 Not found
    * 409 Conflict
    * 410 Gone
    * 411 Length Required
    * 412 Precondition Failed
    * 413 Request Entity Too Large
    * 414 Request-URI Too Long
    * 415 Unsupported Media Type

###### 5XX Server Fail

    * 500 Internal Server Error
    * 501 Not Implemented
    * 502 Bad Gateway
    * 503 Service Unavailable
    * 504 Gateway Timeout
    * 505 HTTP Version Not Supported
    * 506 Variant Also Negotiates
    * 507 Insufficient Storage
    * 510 Not Extended

