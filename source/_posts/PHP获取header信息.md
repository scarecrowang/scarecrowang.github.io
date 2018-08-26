---
title: PHP获取header信息
date: 2017-10-19 13:32:00
tags: 
- php
categories: 
- PHP
---
之前在Egret工作的时候，需要和合作伙伴的接口做对接。某次需要从对方传过来的header信息中获取自己需要的数据，用PHP封装了一个方法，放这里留着以后用。

{% codeblock lang:php %}

function get_head($sUrl){
    $oCurl = curl_init();
    $header[] = "Content-type: application/x-www-form-urlencoded";
    $user_agent = "Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/33.0.1750.146 Safari/537.36";

    curl_setopt($oCurl, CURLOPT_URL, $sUrl);
    curl_setopt($oCurl, CURLOPT_HTTPHEADER,$header);
    curl_setopt($oCurl, CURLOPT_HEADER, true);
    curl_setopt($oCurl, CURLOPT_NOBODY, true);
    curl_setopt($oCurl, CURLOPT_USERAGENT,$user_agent);
    curl_setopt($oCurl, CURLOPT_RETURNTRANSFER, 1 );
    curl_setopt($oCurl, CURLOPT_POST, false);

    $sContent = curl_exec($oCurl);
    $headerSize = curl_getinfo($oCurl, CURLINFO_HEADER_SIZE);
    $header = substr($sContent, 0, $headerSize);

    curl_close($oCurl);

    return $header;
}

//header头信息
$baiduUrl = "http://www.baidu.com/link?url=LZE_J6a1AcieLlTzNxUZQVpe2trQ99zx1ls85ux8dXaGlFB3eiEm_Y6SJC1sNQf_";
$responseHead = get_head($baiduUrl);
print_r('Header:  '.$responseHead.'</br></br></br>');


//获取location
$headArr = explode("\r\n", $responseHead);
foreach ($headArr as $loop) {
    if(strpos($loop, "Location") !== false){
        $edengUrl = trim(substr($loop, 10));
        print_r('Location:   '.$edengUrl);
    }
}
{% endcodeblock %}