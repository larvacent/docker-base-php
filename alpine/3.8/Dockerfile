FROM larvacent/alpine:3.8

LABEL maintainer="support@larvacent.com"

# persistent / runtime deps
ENV PHPIZE_DEPS autoconf dpkg-dev file g++ gcc libc-dev make re2c linux-headers

RUN set -x \
	&& apk add --no-cache --virtual .persistent-deps \
		curl \
		gmp \
		bzip2 \
		freetype \
		tar \
		libressl \
		icu \
		gettext \
		tidyhtml \
		libxml2 \
		libxslt \
		libjpeg-turbo \
		libedit \
		libpng \
		libcurl \
		libbz2 \
		libzip \
	&& addgroup -g 82 -S www-data \
	&& adduser -u 82 -D -S -G www-data www-data
# 82 is the standard uid/gid for "www-data" in Alpine

RUN set -x \
	&& apk add --no-cache --virtual .build-deps \
		$PHPIZE_DEPS \
	&& cd /tmp \
	&& curl -fSL https://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.15.tar.gz -o libiconv-1.15.tar.gz \
	&& tar zxf libiconv-1.15.tar.gz \
	&& rm -rf libiconv-1.15.tar.gz \
	&& cd libiconv-1.15/ \
	&& ./configure --prefix=/usr --enable-static \
	&& make -j "$(nproc)" \
	&& make install \
	&& rm -rf /tmp/libiconv-1.15 \
	&& apk del .build-deps