---
title: PHP证书加密和解密
date: 2019-05-09 18:18:12
tags:
- php
---

### 加密签名
```php

/**
 * 获取签名
 *
 * @param string $str            需要加密的字符串
 * @param string $rsaPrivateFile 私钥文件地址
 *
 * @return string
 */
function getRsaSign($str, $rsaPrivateFile)
{
    $privateKey = file_get_contents($rsaPrivateFile);
    $resource   = openssl_get_privatekey($privateKey);
    openssl_sign($str, $sign, $resource, OPENSSL_ALGO_SHA256);
    openssl_free_key($resource);
    return base64_encode($sign);
}

```

### 秘钥验证

```php
/**
 * 验证签名
 *
 * @param string $sign          需要验证的签名字符串
 * @param string $str           参与加密的字符串
 * @param string $rsaPublicFile 公钥文件地址
 *
 * @return bool
 */
function checkRsaSign($sign, $str, $rsaPublicFile)
{
    $public_key = file_get_contents($rsaPublicFile);
    $resource   = openssl_get_publickey($public_key);
    $result     = (bool)openssl_verify($str, $sign, $resource, OPENSSL_ALGO_SHA256);
    openssl_free_key($resource);
    return $result;
}
```
