

1. 安装openssl

> brew install openssl@1.1

2. 生成自签名证书
>openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /path/to/redis.key -out /path/to/redis.crt
3. 生成ca证书
> openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout ca.key -out ca.crt

4. 修改redis 的配置文件redis.conf （非双向验证）

```
port 0
tls-port 6379
tls-cert-file /path/to/redis.crt
tls-key-file /path/to/redis.key
tls-ca-cert-file /path/to/ca.crt 
tls-cluster yes
tls-replication no
tls-auth-clients optional
tls-protocol "TLSv1.2 TLSv1.3"
tls-cipher-suite DEFAULT:!MEDIUM
```
5. 启动redis-server
> redis-server /path/to/redis.conf

6. 将自签名证书加入到python环境中
>  python -c "import ssl; print(ssl.get_default_verify_paths())"

找到 openssl_capath 路径
> cp /path/to/redis.crt openssl_capath

