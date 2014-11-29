# Contributor: Jean-Sébastien Ney <jeansebastien.ney@gmail.com>
# Contributor: James Cleveland <jc@blackflags.co.uk>
# Maintainer: Jordan Anderson <aq@es.gy>
_cfgdir=/opt/openresty/nginx/conf
_tmpdir=/var/cache/nginx
_passengerver=5.0.0.beta1
_pagespeedver=1.9.32.2

pkgname=nginx-plus
_pkgname=ngx_openresty
pkgver=1.7.4.1
pkgrel=4
pkgdesc="Openresty(+Nginx)+Pagespeed+Passenger all in one sweet package"
arch=('i686' 'x86_64')
url="http://openresty.org/"
license=('BSD')
#install='nginx.install'
depends=('perl>=5.6.1' 'readline' 'pcre' 'openssl' 'zlib' 'ruby' 'curl' 'geoip' 'luajit')
provides=('nginx' 'passenger' 'openresty')
depends=('pcre' 'zlib' 'openssl' 'ruby' 'curl' 'geoip' 'luajit')
optdepends=('python: Support for python web apps' 'nodejs: Support for node.js web apps')
conflicts=('nginx' 'passenger' 'openresty')
source=(http://openresty.org/download/$_pkgname-$pkgver.tar.gz
    https://github.com/phusion/passenger/archive/release-$_passengerver.tar.gz
    https://github.com/pagespeed/ngx_pagespeed/archive/release-$_pagespeedver-beta.zip
    https://dl.google.com/dl/page-speed/psol/$_pagespeedver.tar.gz
        service
        nginx.logrotate
	tmpfiles)
noextract=()
md5sums=('e4b10833beb0fc8e14dbe1ba6af3126e'
         'e1219aa025e290f4bc9b947ff9da9610'
         '27a635042cf6ed0ac76d7d2f5d0edb40'
         '60a6b6e4e3c2fa7aff7fe25150b0f067'
         'ce9a06bcaf66ec4a3c4eb59b636e0dfd'
         'b38744739022876554a0444d92e6603b'
         'f1c71df37cc686bd8b13fa6d36a78156')

backup=(${_cfgdir:1}/fastcgi.conf
        ${_cfgdir:1}/fastcgi_params
        ${_cfgdir:1}/koi-win
        ${_cfgdir:1}/koi-utf
        ${_cfgdir:1}/mime.types
        ${_cfgdir:1}/nginx.conf
        ${_cfgdir:1}/scgi_params
        ${_cfgdir:1}/uwsgi_params
        ${_cfgdir:1}/win-utf
        etc/logrotate.d/nginx
        etc/nginx/nginx.conf
        etc/nginx/fastcgi.conf)

build() {

  ln -sf "$srcdir"/psol "$srcdir"/ngx_pagespeed-release*
    cd "$srcdir/passenger-release-$_passengerver"
    _nginx_addon_dir=`bin/passenger-config --nginx-addon-dir`

    export LUAJIT_LIB=/usr/lib/lua/5.1
    export LUAJIT_INC=/usr/include/luajit-2.0

  cd "$srcdir/$_pkgname-$pkgver"

  ./configure \
        --prefix=/opt/openresty \
        --pid-path=/run/nginx.pid \
        --lock-path=/run/lock/nginx.lock \
        --conf-path=/opt/openresty/nginx/conf/nginx.conf \
        --http-client-body-temp-path=/var/cache/nginx/client-body \
        --http-proxy-temp-path=/var/cache/nginx/proxy \
        --http-fastcgi-temp-path=/var/cache/nginx/fcgi \
        --http-scgi-temp-path=/var/cache/nginx/scgi \
        --http-uwsgi-temp-path=/var/cache/nginx/uwsgi \
        --sbin-path=/usr/bin/nginx \
        --user=http \
        --group=http \
        --with-imap \
        --with-imap_ssl_module \
        --with-ipv6 \
        --with-pcre-jit \
        --with-luajit \
        --with-file-aio \
        --with-http_dav_module \
        --with-http_geoip_module \
        --with-http_gunzip_module \
        --with-http_gzip_static_module \
        --with-http_realip_module \
        --with-http_spdy_module \
        --with-http_image_filter_module \
        --with-http_ssl_module \
        --with-http_stub_status_module \
        --with-http_addition_module \
        --with-http_degradation_module \
        --with-http_flv_module \
        --with-http_mp4_module \
        --with-http_secure_link_module \
        --with-http_sub_module \
        --add-module="$_nginx_addon_dir" \
        --add-module="../ngx_pagespeed-release-$_pagespeedver-beta" \
    # --without-http_echo_module         \ # disable ngx_http_echo_module
    # --without-http_xss_module          \ # disable ngx_http_xss_module
    # --without-http_coolkit_module      \ # disable ngx_http_coolkit_module
    # --without-http_set_misc_module     \ # disable ngx_http_set_misc_module
    # --without-http_form_input_module   \ # disable ngx_http_form_input_module
    # --without-http_encrypted_session_module \
    #                                    \ # disable ngx_http_encrypted_session_module
    # --without-http_srcache_module      \ # disable ngx_http_srcache_module
    # --without-http_lua_module          \ # disable ngx_http_lua_module
    # --without-http_headers_more_module \ # disable ngx_http_headers_more_module
    # --without-http_array_var_module    \ # disable ngx_http_array_var_module
    # --without-http_memc_module         \ # disable ngx_http_memc_module
    # --without-http_redis2_module       \ # disable ngx_http_redis2_module
    # --without-http_redis_module        \ # disable ngx_http_redis_module
    # --without-http_auth_request_module \ # disable ngx_http_auth_request_module
    # --without-http_rds_json_module     \ # disable ngx_http_rds_json_module
    # --without-http_rds_csv_module      \ # disable ngx_http_rds_csv_module
    # --without-ngx_devel_kit_module     \ # disable ngx_devel_kit_module
    # --without-http_ssl_module          \ # disable ngx_http_ssl_module
    # --with-http_iconv_module           \ # enable ngx_http_iconv_module
    # --with-http_drizzle_module         \ # enable ngx_http_drizzle_module
    # --with-http_postgres_module        \ # enable ngx_http_postgres_module
    # --without-lua_cjson                \ # disable the lua-cjson library
    # --without-lua_redis_parser         \ # disable the lua-redis-parser library
    # --without-lua_rds_parser           \ # disable the lua-rds-parser library
    # --without-lua_resty_dns            \ # disable the lua-resty-dns library
    # --without-lua_resty_memcached      \ # disable the lua-resty-memcached library
    # --without-lua_resty_redis          \ # disable the lua-resty-redis library
    # --without-lua_resty_mysql          \ # disable the lua-resty-mysql library
    # --without-lua_resty_upload         \ # disable the lua-resty-upload library
    # --without-lua_resty_string         \ # disable the lua-resty-string library
    # --without-lua51                    \ # disable the bundled Lua 5.1 interpreter
    # --with-lua51=PATH                  \ # specify the external installation of Lua 5.1 by PATH
    # --with-luajit                      \ # enable and build LuaJIT 2.0
    # --with-luajit=PATH                 \ # use the external LuaJIT 2.0 installation specified by PATH
    # --with-luajit-xcflags=FLAGS        \ # Specify extra C compiler flags for LuaJIT 2.0
    # --with-libdrizzle=DIR              \ # specify the libdrizzle 1.0 (or drizzle) installation prefix
    # --with-libpq=DIR                   \ # specify the libpq (or postgresql) installation prefix
    # --with-pg_config=PATH              \ # specify the path of the pg_config utility
    # \ # Options directly inherited from nginx
    # --sbin-path=PATH                   \ # set nginx binary pathname
    # --conf-path=PATH                   \ # set nginx.conf pathname
    # --error-log-path=PATH              \ # set error log pathname
    # --pid-path=PATH                    \ # set nginx.pid pathname
    # --lock-path=PATH                   \ # set nginx.lock pathname
    # --tapset-prefix=PATH               \ # set systemtap tapset directory prefix
    # --stap-nginx-path=PATH             \ # set stap-nginx pathname
    # --user=USER                        \ # set non-privileged user for
    #                                    \ # worker processes
    # --group=GROUP                      \ # set non-privileged group for
    #                                    \ # worker processes
    # --builddir=DIR                     \ # set the build directory
    # --with-rtsig_module                \ # enable rtsig module
    # --with-select_module               \ # enable select module
    # --without-select_module            \ # disable select module
    # --with-poll_module                 \ # enable poll module
    # --without-poll_module              \ # disable poll module
    # --with-file-aio                    \ # enable file aio support
    # --with-ipv6                        \ # enable ipv6 support
    # --with-http_realip_module          \ # enable ngx_http_realip_module
    # --with-http_addition_module        \ # enable ngx_http_addition_module
    # --with-http_xslt_module            \ # enable ngx_http_xslt_module
    # --with-http_image_filter_module    \ # enable ngx_http_image_filter_module
    # --with-http_geoip_module           \ # enable ngx_http_geoip_module
    # --with-http_sub_module             \ # enable ngx_http_sub_module
    # --with-http_dav_module             \ # enable ngx_http_dav_module
    # --with-http_flv_module             \ # enable ngx_http_flv_module
    # --with-http_gzip_static_module     \ # enable ngx_http_gzip_static_module
    # --with-http_random_index_module    \ # enable ngx_http_random_index_module
    # --with-http_secure_link_module     \ # enable ngx_http_secure_link_module
    # --with-http_degradation_module     \ # enable ngx_http_degradation_module
    # --with-http_stub_status_module     \ # enable ngx_http_stub_status_module
    # --without-http_charset_module      \ # disable ngx_http_charset_module
    # --without-http_gzip_module         \ # disable ngx_http_gzip_module
    # --without-http_ssi_module          \ # disable ngx_http_ssi_module
    # --without-http_userid_module       \ # disable ngx_http_userid_module
    # --without-http_access_module       \ # disable ngx_http_access_module
    # --without-http_auth_basic_module   \ # disable ngx_http_auth_basic_module
    # --without-http_autoindex_module    \ # disable ngx_http_autoindex_module
    # --without-http_geo_module          \ # disable ngx_http_geo_module
    # --without-http_map_module          \ # disable ngx_http_map_module
    # --without-http_split_clients_module \ 
    #                                    \ # disable ngx_http_split_clients_module
    # --without-http_referer_module      \ # disable ngx_http_referer_module
    # --without-http_rewrite_module      \ # disable ngx_http_rewrite_module
    # --without-http_proxy_module        \ # disable ngx_http_proxy_module
    # --without-http_fastcgi_module      \ # disable ngx_http_fastcgi_module
    # --without-http_uwsgi_module        \ # disable ngx_http_uwsgi_module
    # --without-http_scgi_module         \ # disable ngx_http_scgi_module
    # --without-http_memcached_module    \ # disable ngx_http_memcached_module
    # --without-http_limit_zone_module   \ # disable ngx_http_limit_zone_module
    # --without-http_limit_req_module    \ # disable ngx_http_limit_req_module
    # --without-http_empty_gif_module    \ # disable ngx_http_empty_gif_module
    # --without-http_browser_module      \ # disable ngx_http_browser_module
    # --without-http_upstream_ip_hash_module \
    #                                    \ # disable ngx_http_upstream_ip_hash_module
    # --with-http_perl_module            \ # enable ngx_http_perl_module
    # --with-perl_modules_path=PATH      \ # set path to the perl modules
    # --with-perl=PATH                   \ # set path to the perl binary
    # --http-log-path=PATH               \ # set path to the http access log
    # --http-client-body-temp-path=PATH  \ # set path to the http client request body
    #                                    \ # temporary files
    # --http-proxy-temp-path=PATH        \ # set path to the http proxy temporary files
    # --http-fastcgi-temp-path=PATH      \ # set path to the http fastcgi temporary
    #                                    \ # files
    # --http-uwsgi-temp-path=PATH        \ # set path to the http uwsgi temporary files
    # --http-scgi-temp-path=PATH         \ # set path to the http scgi temporary files
    # --without-http                     \ # disable HTTP server
    # --without-http-cache               \ # disable HTTP cache
    # --with-mail                        \ # enable POP3/IMAP4/SMTP proxy module
    # --with-mail_ssl_module             \ # enable ngx_mail_ssl_module
    # --without-mail_pop3_module         \ # disable ngx_mail_pop3_module
    # --without-mail_imap_module         \ # disable ngx_mail_imap_module
    # --without-mail_smtp_module         \ # disable ngx_mail_smtp_module
    # --with-google_perftools_module     \ # enable ngx_google_perftools_module
    # --with-cpp_test_module             \ # enable ngx_cpp_test_module
    # --add-module=PATH                  \ # enable an external module
    # --with-cc=PATH                     \ # set path to C compiler
    # --with-cpp=PATH                    \ # set path to C preprocessor
    # --with-cc-opt=OPTIONS              \ # set additional options for C compiler
    # --with-ld-opt=OPTIONS              \ # set additional options for linker
    # --with-cpu-opt=CPU                 \ # build for specified CPU, the valid values:
    #                                    \ # pentium, pentiumpro, pentium3, pentium4,
    #                                    \ # athlon, opteron, sparc32, sparc64, ppc64
    # --with-make=PATH                   \ # specify the default make utility to be used
    # --without-pcre                     \ # disable PCRE library usage
    # --with-pcre                        \ # force PCRE library usage
    # --with-pcre=DIR                    \ # set path to PCRE library sources
    # --with-pcre-opt=OPTIONS            \ # set additional options for PCRE building
    # --with-pcre-jit                    \ # build PCRE with JIT compilation support
    # --with-md5=DIR                     \ # set path to md5 library sources
    # --with-md5-opt=OPTIONS             \ # set additional options for md5 building
    # --with-md5-asm                     \ # use md5 assembler sources
    # --with-sha1=DIR                    \ # set path to sha1 library sources
    # --with-sha1-opt=OPTIONS            \ # set additional options for sha1 building
    # --with-sha1-asm                    \ # use sha1 assembler sources
    # --with-zlib=DIR                    \ # set path to zlib library sources
    # --with-zlib-opt=OPTIONS            \ # set additional options for zlib building
    # --with-zlib-asm=CPU                \ # use zlib assembler sources optimized
    #                                    \ # for specified CPU, the valid values:
    #                                    \ # pentium, pentiumpro
    # --with-libatomic                   \ # force libatomic_ops library usage
    # --with-libatomic=DIR               \ # set path to libatomic_ops library sources
    # --with-openssl=DIR                 \ # set path to OpenSSL library sources
    # --with-openssl-opt=OPTIONS         \ # set additional options for OpenSSL building
    # --dry-run                          \ # dry running the configure, for testing only
    # --platform=PLATFORM                \ # forcibly specify a platform name, for testing only

  make -j4

}

package() {
  cd "$srcdir/$_pkgname-$pkgver"
  make DESTDIR="$pkgdir" install
  install -d "$pkgdir"/etc/logrotate.d
  install -m644 "$srcdir"/nginx.logrotate "$pkgdir"/etc/logrotate.d/nginx
  install -d "$pkgdir"/$_tmpdir
  install -Dm644 "$srcdir/service" "$pkgdir/usr/lib/systemd/system/nginx.service"
  install -Dm644 "$srcdir/tmpfiles" "$pkgdir/usr/lib/tmpfiles.d/nginx.conf"

  rm "$pkgdir/opt/openresty/nginx/conf/fastcgi.conf"
  rm "$pkgdir/opt/openresty/nginx/conf/fastcgi_params"
  rm "$pkgdir/opt/openresty/nginx/conf/fastcgi_params.default"
  rm "$pkgdir/opt/openresty/nginx/conf/nginx.conf"
  rm -rf "$pkgdir/var/run"

    #Passenger
    cd "$srcdir/passenger-release-$_passengerver"

    #Passenger bin
    install -d "$pkgdir"/usr/lib/passenger/buildout
    rm -r $(find . -name "Makefile" -or -name "*.o")
    cp -R buildout/{support-binaries,ruby} "$pkgdir"/usr/lib/passenger/buildout
    cp -R {bin,helper-scripts,lib,node_lib,resources} "$pkgdir"/usr/lib/passenger/

    #Passenger native support
    install -d "$pkgdir"/usr/lib/passenger/ext
    cp -R ext/ruby "$pkgdir"/usr/lib/passenger/ext/

    #Passenger doc
    install -d "$pkgdir"/usr/share/doc/passenger
    cp -R doc "$pkgdir"/usr/share/doc/passenger/

    #Passenger man
    install -Dm644 man/passenger-config.1 "$pkgdir"/usr/share/man/man1/passenger-config.1
    #install -Dm644 man/passenger-stress-test.1 "$pkgdir"/usr/share/man/man1/passenger-stress-test.1
    install -Dm644 man/passenger-memory-stats.8 "$pkgdir"/usr/share/man/man8/passenger-memory-stats.8
    install -Dm644 man/passenger-status.8 "$pkgdir"/usr/share/man/man8/passenger-status.8

}
# vim:set ts=2 sw=2 et:
