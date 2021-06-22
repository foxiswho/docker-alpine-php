
# 配合 docker composer 使用
https://github.com/foxiswho/docker-compose-nginx-php-mysql


# 本地构建
```SHEL
docker build . -f Dockerfile -t alphp/php80:base
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


echo "https://mirror.tuna.tsinghua.edu.cn/alpine/edge/main" > /etc/apk/repositories \
&& echo "https://mirror.tuna.tsinghua.edu.cn/alpine/edge/community" >> /etc/apk/repositories \
&& echo "https://mirror.tuna.tsinghua.edu.cn/alpine/edge/testing" >> /etc/apk/repositories

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
