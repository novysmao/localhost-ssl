# localhost-ssl
## 创建CA证书
1.创建一个秘钥，这个便是CA证书的根本，之后所有的东西都来自这个秘钥  
<code>openssl genrsa -out myCA.key 2048</code>  
2.通过秘钥加密机构信息形成公钥  
<-- Common Name填localhost  -->  
openssl req -new -x509 -key myCA.key -out myCA.cer -days 36500  

## 创建服务器证书
1.创建服务器的秘钥  
<code>openssl genrsa -out server.key 2048</code>  
2.创建一个签名配置文件openssl.cnf  
3.将上面的配置内容保存为openssl.cnf放到生成的服务器证书文件的目录下（注意：修改alt_names里面的域名或者IP为最终部署需要的地址，支持通配符），然后执行创建签名申请文件即可，执行运行：  
<code>openssl req -config openssl.cnf -new -out server.req -key server.key</code>  
4.通过CA机构证书对服务器证书进行签名认证  
<code>openssl x509 -req  -extfile openssl.cnf -extensions v3_req -in server.req -out server.cer -CAkey myCA.key -CA myCA.cer -days 36500 -CAcreateserial -CAserial serial</code>  
5.部署证书  
server.key就是秘钥，server.cer文件就是公钥,只需要配置给服务器就行了

## 安装证书
将CA证书的公钥（myCA.cer文件）导入到系统信任的根证书颁发机构里面
