# API Test script (PHP) #

You can find below the script that could help you to integrate the API with your own application.  
It provides a clear view of what's sent back by the API, and also provide the response time.   

**Caution : You should not use directly this script into your code.**

```php
<?php

#########################################################################################
#
# API Testing Script for FX4BIZ
#
# @Author Adrien Gras <agr@fxforbiz.com>
#
# This script is meant for testing purpose. You should not use directly into your code.
# You can executing it by filling the needed informations below, and then with a shell, 
# call it by using :
#
# 						$ php <path_to_this_script>/testAPI.php
#
#########################################################################################

#########################################################################################
# REQUEST INFORMATIONS
#########################################################################################

# $username 	: The username used to connect to the API.
$username 		= "<login>";

# $password 	: The password used to connect to the API.
$password 		= "<password>";

# $url 			: The url to call the API.
$url 			= "<URL>";

# $method 		: The HTTP method of the request.
$method 		= "<HTTP method>";

# $postParams  	: The JSON Object to send with a POST request.
$postParams 	=<<<JSON
{
	"some_json":"here"
}
JSON;

#########################################################################################
# REQUEST PROCESS CODE (do not modify this part)
#########################################################################################

echo "### TEST API ###\n\n";

$nonce="";$nonce64;$date;$digest;$header;

$chars = "0123456789abcdef";
for ($i = 0; $i < 32; $i++) {
    $nonce .= $chars[rand(0, 15)];
}
$nonce64 = base64_encode($nonce) ;
$date = gmdate('c');
$date = substr($date,0,19)."Z" ;
$digest = base64_encode(sha1($nonce.$date.$password, true));
$header = sprintf('X-WSSE: UsernameToken Username="%s", PasswordDigest="%s", Nonce="%s", Created="%s"',$username, $digest, $nonce64, $date);

echo " - User : ".$username."\n";
echo " - Path : ".$method." ".$url."\n";
echo " - Header : ".$header."\n\n";
echo "################\n\n";

$ch = curl_init();
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_HEADER, 1);
if($method == "GET"){
	curl_setopt($ch, CURLOPT_HTTPGET, 1);
}
if($method == "POST"){
	curl_setopt($ch, CURLOPT_POST, 1);
	curl_setopt($ch, CURLOPT_POSTFIELDS, $postParams);
}
if($method == "DELETE"){
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "DELETE");
}
if($method == "PUT"){
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "PUT");
}
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch,CURLOPT_HTTPHEADER, array($header,'Content-Type: application/json'));

$time_start = microtime(true);

$res = curl_exec($ch);

$time_end = microtime(true);
$time = $time_end - $time_start;

curl_close($ch);

$firstline = explode("\r\n",$res)[0];
$json = json_encode(json_decode(explode("\r\n\r\n",$res)[1], true), JSON_PRETTY_PRINT | JSON_UNESCAPED_SLASHES);
echo $firstline."\n\n".$json."\n\n";
echo (round($time,3)*1000)." ms\n\n";
echo "################\n\n";

# EOF

```