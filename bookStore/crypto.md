## crypto模块

目的是为了提供通用的加密和哈希算法。用纯JavaScript代码实现这些功能不是不可能，但速度会非常慢。`Nodejs`用`C/C++`实现这些算法后，通过`cypto`这个模块暴露为JavaScript接口，这样用起来方便，运行速度也快。

## MD5和SHA1

`MD5`是一种常用的哈希算法，用于给任意数据一个“签名”。这个签名通常用一个十六进制的字符串表示：

    const crypto = require('crypto');

    const hash = crypto.createHash('md5');

    // 可任意多次调用update():
    hash.update('Hello, world!');
    hash.update('Hello, nodejs!');

## RSA

RSA算法是一种非对称加密算法，即由一个私钥和一个公钥构成的密钥对，通过私钥加密，公钥解密，或者通过公钥加密，私钥解密。其中，公钥可以公开，私钥必须保密。

## AES

`AES`是一种常用的对称加密算法，加解密都用同一个密钥。`crypto`模块提供了`AES`支持，但是需要自己封装好函数，便于使用：

    const crypto = require('crypto');

    //加密
    function aesEncrypt(data, key) {
        const cipher = crypto.createCipher('aes192', key);
        var crypted = cipher.update(data, 'utf8', 'hex');
        crypted += cipher.final('hex');
        return crypted;
    }

    //解密
    function aesDecrypt(encrypted, key) {
        const decipher = crypto.createDecipher('aes192', key);
        var decrypted = decipher.update(encrypted, 'hex', 'utf8');
        decrypted += decipher.final('utf8');
        return decrypted;
    }

    var data = 'happy everyday';
    var key = 'secertKey';
    var encrypted = aesEncrypt(data, key);
    var decrypted = aesDecrypt(encrypted, key);

    console.log('Plain text: ' + data); // Plain text: happy everyday
    console.log('Encrypted text: ' + encrypted); // Encrypted text: 8a944d97bdabc157a5b7a40cb180e7...
    console.log('Decrypted text: ' + decrypted); // Plain text: happy everyday


注意到`AES`有很多不同的算法，如`aes192，aes-128-ecb，aes-256-cbc`等，`AES`除了密钥外还可以指定`IV（Initial Vector）`，不同的系统只要`IV`不同，用相同的密钥加密相同的数据得到的加密结果也是不同的。加密结果通常有两种表示方法：`hex`和`base64`，这些功能`Nodejs`全部都支持，但是在应用中要注意，如果加解密双方一方用`Nodejs，`另一方用`Java、PHP`等其它语言，需要仔细测试。如果无法正确解密，要确认双方是否遵循同样的`AES`算法，字符串密钥和`IV`是否相同，加密后的数据是否统一为`hex`或`base64`格式。


## 证书

`crypto`模块也可以处理数字证书。数字证书通常用在SSL连接，也就是Web的https连接。一般情况下，`https`连接只需要处理服务器端的单向认证，如无特殊需求（例如自己作为`Root`给客户发认证证书），建议用反向代理服务器如`Nginx`等`Web`服务器去处理证书。