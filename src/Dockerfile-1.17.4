FROM docker.io/openmage/debian:stretch-latest

COPY root /

## configure default environment stuff and file permissions
RUN set -xe; \
    \
        ## block packages from being installed
    	{ \
    		echo 'Package: nginx*'; \
    		echo 'Pin: release *'; \
    		echo 'Pin-Priority: -1'; \
    	} > /etc/apt/preferences.d/no-nginx;

ARG NGINX_RUNTIME_REQUIREMENTS="curl zlib1g"
ARG NGINX_RUNTIME_REQUIREMENTS_EXTRA=""
ARG NGINX_BUILD_REQUIREMENTS="build-essential libpcre3-dev libbz2-dev libssl1.0-dev zlib1g-dev git libbrotli-dev"
ARG NGINX_BUILD_REQUIREMENTS_EXTRA=""
ARG NGINX_EXTRA_CONFIGURE_ARGS=""
ARG NGINX_VERSION="1.17.4"

## configure nginx and the dependencies
RUN set -xe; \
    \
    /usr/local/bin/docker-install-requirements nginx; \
    mkdir -p /var/cache/nginx; \
    chown -R www-data:www-data /var/cache/nginx/; \
    git clone https://github.com/google/ngx_brotli; \
    cd /tmp/ngx_brotli/; \
    git submodule update --init; \
    cd /tmp/; \
    mkdir -p /tmp/nginx; \
    cd /tmp/nginx; \
    docker-package-download -o nginx.tar.gz -s http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz; \
    tar --strip 1 -xzf nginx.tar.gz; \
    ./configure \
        --prefix=/usr/ \
        --sbin-path=/usr/sbin/nginx \
        --modules-path=/usr/lib/nginx/modules \
        --conf-path=/etc/nginx/nginx.conf \
        --error-log-path=/var/log/nginx/error.log \
        --http-log-path=/var/log/nginx/access.log \
        --pid-path=/var/run/nginx.pid \
        --lock-path=/var/run/nginx.lock \
        --http-client-body-temp-path=/var/cache/nginx/client_temp \
        --http-proxy-temp-path=/var/cache/nginx/proxy_temp \
        --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp \
        --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp \
        --http-scgi-temp-path=/var/cache/nginx/scgi_temp \
        --user=www-data \
        --group=www-data \
        --with-compat \
        --with-file-aio \
        --with-threads \
        --with-http_addition_module \
        --with-http_auth_request_module \
        --with-http_flv_module \
        --with-http_gunzip_module \
        --with-http_gzip_static_module \
        --with-http_sub_module \
        --with-stream \
        --with-stream_realip_module \
        --with-stream_ssl_module \
        --with-stream_ssl_preread_module \
        --with-http_mp4_module \
        --with-http_realip_module \
        --with-http_secure_link_module \
        --with-http_ssl_module \
        --with-http_stub_status_module  \
        --with-http_v2_module \
        --without-mail_pop3_module \
        --without-mail_imap_module \
        --without-mail_smtp_module \
        --without-http_uwsgi_module \
        --without-http_scgi_module \
        --without-http_memcached_module \
        --with-http_realip_module \
        --add-module=/tmp/ngx_brotli \
        ${NGINX_EXTRA_CONFIGURE_ARGS:-} \
        ; \
        make -j$(nproc); \
        make install; \
        # forward request and error logs to docker log collector
        ln -sf /dev/stdout /var/log/nginx/access.log; \
        ln -sf /dev/stderr /var/log/nginx/error.log; \
        mkdir -p /etc/nginx/site.d; \
        mkdir -p /etc/nginx/conf.d; \
        find /etc/nginx -type d -exec chmod 755 {} \; ;\
        find /etc/nginx -type f -exec chmod 755 {} \; ;\
    /usr/local/bin/docker-layer-clean

# set nginx healthcheck
HEALTHCHECK --start-period=10s --interval=10s --timeout=1s CMD curl --fail http://127.0.0.1/nginx-status/ || exit 1

# Set stop signal
STOPSIGNAL SIGQUIT

# Set work dir
WORKDIR /var/www/html

# Expose ports
EXPOSE 80 443

# Default command
CMD ["nginx", "-g", "daemon off;"]
