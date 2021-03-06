
# 配合 docker composer 使用
https://github.com/foxiswho/docker-compose-nginx-php-mysql


# 本地构建
如果你需要本地构建，那么先构建`base`目录下的，
然后修改 `fpm`目录下`Dockerfile`中的 `FROM foxiswho/php-alpine:base-php71`修改为`FROM alphp/php71:base`
再执行如下命令构建
```SHEL
docker build . -f Dockerfile -t alphp/php71:fpm
```

# 自行安装扩展
进入内部执行
```SHEL
apk update

apk add php8-pecl-solr
```

php8 扩展搜索

https://pkgs.alpinelinux.org/packages?page=1&branch=edge&name=php8-%2A&arch=x86_64

php8 扩展搜索

https://pkgs.alpinelinux.org/packages?page=1&branch=edge&name=php8-pecl-%2A&arch=x86_64


# PHP MODULE

```bash
pear 
fpm 
bz2 
bcmath 
calendar 
curl 
cgi 
ctype 
dom 
dba 
doc 
embed 
enchant 
exif 
ftp 
fileinfo 
gd 
gettext 
gmp 
iconv 
imap 
intl 
ldap 
mbstring 
mongodb 
mysqlnd 
mysqli 
odbc 
openssl 
opcache 
pcntl 
pdo 
pdo_dblib 
pdo_mysql 
pdo_odbc 
pdo_pgsql 
pdo_sqlite 
pgsql 
phar 
phpdbg 
pspell 
posix
simplexml
session 
shmop 
snmp 
sockets 
soap 
sodium 
sqlite3
sysvshm 
sysvmsg 
sysvsem 
tidy 
tokenizer
xml 
xmlreader 
xmlwriter 
xsl 
zip
pecl-redis  
pecl-memcached  
pecl-mongodb  
pecl-swoole  
pecl-imagick  
pecl-event  
pecl-protobuf  
pecl-xhprof  
pecl-ssh2  
pecl-csv  
pecl-ssh2  
pecl-decimal  
pecl-timezonedb  
pecl-xmlrpc  
pecl-seaslog  
pecl-xlswriter  
pecl-yaml  
pecl-uuid  
pecl-mcrypt  
pecl-grpc  
```
