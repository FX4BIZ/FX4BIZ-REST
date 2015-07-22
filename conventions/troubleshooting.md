# Troubleshooting #

If you have any issue, please check the list below to see if it's listed. If it's not, please contact our IT team in order to solve it.

## 1. I do requests, but the API take 10 seconds to respond. ##

It is due to a wrong value for `Content-Length` in your HTTP headers. In fact, if you do not specify the correct content length in the `Content-length` HTTP header, our server will wait 10 seconds for a next message specified by the wrong message length.  
To solve this, just remove the `Content-Length` header from your HTTP headers, and things should be fine again.