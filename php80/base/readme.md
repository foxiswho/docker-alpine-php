
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
