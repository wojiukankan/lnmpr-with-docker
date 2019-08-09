FROM php737

ENV NGINX_VERSION 1.16.0

RUN set -eux; \
    apk update ;\
    apk add --no-cache --virtual .build-deps \
    gcc \
    g++ \
    libc-dev \
    make \
    openssl-dev \
    pcre-dev \
    zlib-dev \
    linux-headers \
    libxslt-dev \
    gd-dev \
    geoip-dev \
    perl-dev \
    libedit-dev \
    mercurial \
    bash \
    alpine-sdk \
    findutils \
&& cd ~ \
&& wget -O nginx.tar.gz https://nginx.org/download/nginx-$NGINX_VERSION.tar.gz \
&& tar -zxvf nginx.tar.gz \
&& cd nginx-$NGINX_VERSION \
&& ./configure --user=www-data --group=www-data --with-http_ssl_module --with-http_stub_status_module --with-http_gzip_static_module --with-http_v2_module  --with-http_realip_module --with-http_flv_module --with-http_mp4_module --with-pcre-jit \
&& 	make -j "$(nproc)"  && make install && make clean \
&& apk del --no-network .build-deps \
&& mkdir /usr/local/nginx/conf/conf.d \
&& cd / && rm -rf ~/nginx*

COPY nginx.conf /usr/local/nginx/conf/
COPY default.conf /usr/local/nginx/conf/conf.d/
COPY index.php /usr/local/nginx/html/

WORKDIR /usr/local/nginx/html

STOPSIGNAL SIGQUIT

EXPOSE 80

CMD /usr/local/nginx/sbin/nginx && php-fpm