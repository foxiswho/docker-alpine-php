
# 配合 docker composer 使用
https://github.com/foxiswho/docker-compose-nginx-php-mysql


# 本地构建
如果你需要本地构建，那么先构建`base`目录下的，
然后修改 `fpm`目录下`Dockerfile`中的 `FROM foxiswho/php-alpine:base-php73`修改为`FROM alphp/php71:base`
再执行如下命令构建
```SHEL
docker build . -f Dockerfile -t alphp/php73:fpm
```


# PHP Modules
bcmath
bz2
calendar
Core
ctype
curl
date
dom
exif
fileinfo
filter
ftp
gettext
gmp
hash
iconv
imap
json
libxml
mbstring
mcrypt
memcached
mongodb
mysqli
mysqlnd
odbc
openssl
pcntl
pcre
PDO
pdo_dblib
pdo_mysql
PDO_ODBC
pdo_pgsql
pdo_sqlite
pgsql
Phar
posix
readline
redis
Reflection
session
shmop
SimpleXML
snmp
soap
sockets
SPL
sqlite3
ssh2
standard
swoole
sysvmsg
sysvsem
sysvshm
tidy
tokenizer
xml
xmlreader
xmlwriter
xsl
Zend OPcache
zip
zlib

# Zend Modules
Zend OPcache
