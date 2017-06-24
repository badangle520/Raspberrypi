#Nginx搭建HTTPS服务器
##HTTPS简介
**HTTPS(Hypertext Transfer Protocol over Secure Socket Layer)**,是以安全为目标的HTTP通道，简单来讲就是HTTP的安全版。即HTTP下加入SSL层，HTTPS的安全基础是SSL，因此加密的详细内容就需要SSL。

它是一个URI scheme（抽象标识符体系），句法类同http:体系，用于安全的http数据传输。https使用的默认端口是443.

##HTTPS和HTTP的区别
超文本传输协议HTTP协议被用于在Web浏览器和网站服务器之间传递信息。HTTP协议以明文方式发送内容，不提供任何方式的数据加密，如果攻击者截取了Web浏览器和网站服务器之间的传输报文，就可以直接读懂其中的信息，因此HTTP协议不适合传输一些敏感信息，比如信用卡号、密码等。
为了解决HTTP协议的这一缺陷，需要使用另一种协议：安全套接字层超文本传输协议HTTPS。为了数据传输的安全，HTTPS在HTTP的基础上加入了SSL协议，SSL依靠证书来验证服务器的身份，并为浏览器和服务器之间的通信加密。

HTTPS和HTTP的区别主要为以下四点：

1. https协议需要到ca申请证书，一般免费证书很少，需要交费。
2. http是超文本传输协议，信息是明文传输，https 则是具有安全性的ssl加密传输协议。
3. http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。
4. http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。

##SSL证书
证书类型简介

要设置安全服务器，使用公共钥创建一对公私钥对。大多数情况下，发送证书请求（包括自己的公钥），你的公司证明材料以及费用到一个证书颁发机构(CA).CA验证证书请求及您的身份，然后将证书返回给您的安全服务器。

但是内网实现一个服务器端和客户端传输内容的加密，可以自己给自己颁发证书，只需要忽略掉浏览器不信任的警报即可！

由CA签署的证书为您的服务器提供两个重要的功能：

- 浏览器会自动识别证书并且在不提示用户的情况下允许创建一个安全连接
- 当一个CA生成一个签署过的证书，它为提供网页给浏览器的组织提供身份担保。
- 多数支持ssl的web服务器都有一个CA列表，它们的证书会被自动接受。当一个浏览器遇到一个其授权CA并不在列表中的证书，浏览器将询问用户是否接受或拒绝连接.

##SSL证书生成及NGINX配置
###生成证书
**命令:**

`openssl req -new -sha256 -newkey rsa:2048 -nodes -keyout WWW.XXX.COM.key -out WWW.XXX.COM..csr`

**结果:**

```
Generating RSA private key, 2048 bit long modulus
.....+++
.+++
e is 65537 (0x10001)
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----

Country Name (2 letter code) [AU]:CN                        #国家
State or Province Name (full name) [Some-State]:GuangDong   #省
Locality Name (eg, city) []:ShenZhen                        #城市
Organization Name (eg, company) [Internet Widgits Pty Ltd]: #组织或公司名称
Organizational Unit Name (eg, section) []:                  #可以不填
Common Name (eg, YOUR name) []:www.xxxx.com                 #ssl域名
Email Address []:xxxxxxxx@qq.com                            #邮箱

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:                                    #可以不用填
An optional company name []:                                #可以不用填
```

###创建一个自己签署的CA证书

**命令:**

`openssl req -new -x509 -days 3650 -key www.xxxx.com.key -out www.xxxx.com.crt`

**结果:**

```
Country Name (2 letter code) [AU]:CN                        #输入国家简写
State or Province Name (full name) [Some-State]:GuangDong   #省
Locality Name (eg, city) []:ShenZhen                        #城市
Organization Name (eg, company) [Internet Widgits Pty Ltd]: #组织或公司名称
Organizational Unit Name (eg, section) []:                  #可以不填
Common Name (eg, YOUR name) []:www.xxxx.com                 #ssl域名
Email Address []:xxxxxxxx@qq.com                            #输入邮箱
```

##Nginx部署

**编辑 sites-available/default 文件**
`sudo nano /etc/nginx/sites-available/default`

**内容修改如下:**

```
server {
    listen          443 ssl;
    server_name     www.xxxx.com;
    root            /home/pi/www;         #根目录
    index           index.html index.htm index.php;

    ssl_certificate             /home/pi/ssl/www.xxxx.com.crt;   #证书的位置
    ssl_certificate_key         /home/pi/ssl/www.xxxx.com****.key;   #密钥的位置
    ssl_ciphers                 "EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5";
    ssl_protocols               TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers   on;
    ssl_session_cache           shared:SSL:5m;

    location / {
        autoindex               on;
        autoindex_exact_size    off;
        autoindex_localtime on;
    }

    location ~ \.(jpg|png|gif|js|css|swf|flv|ico)$ {
        expires 12h;
    }

    location ~* ^/(doc|logs|app|sys)/{
        return 403;
    }

    location ~ .*\.(php|php5)?$ {
        fastcgi_pass    unix:/run/php5-fpm.sock;
        fastcgi_index   index.php;
        include         fastcgi.conf;
    }
}

```


**参考文献**

- _http://www.cnblogs.com/grimm/p/5938511.html_

- _https://www.vpser.net/build/letsencrypt-free-ssl.html_

- _https://www.vpser.net/manage/namecheap-free-ssl-nginx.html_


