FROM alpine:3.21

ENV OTP_VERSION="27.2.3"

LABEL org.opencontainers.image.version=$OTP_VERSION

RUN set -xe \
    && OTP_DOWNLOAD_URL="https://github.com/erlang/otp/archive/OTP-${OTP_VERSION}.tar.gz" \
    && OTP_DOWNLOAD_SHA256="9a35cd7c739260c4c5653350f2a76e41a2e9d6fd6124c07f620202cccb91f8f7" \
    && apk add --no-cache --virtual .fetch-deps \
    curl \
    ca-certificates \
    && curl -fSL -o otp-src.tar.gz "$OTP_DOWNLOAD_URL" \
    && echo "$OTP_DOWNLOAD_SHA256  otp-src.tar.gz" | sha256sum -c - \
    && apk add --no-cache --virtual .build-deps \
    dpkg-dev dpkg \
    gcc \
    g++ \
    libc-dev \
    linux-headers \
    make \
    autoconf \
    ncurses-dev \
    openssl-dev \
    unixodbc-dev \
    lksctp-tools-dev \
    tar \
    && export ERL_TOP="/usr/src/otp_src_${OTP_VERSION%%@*}" \
    && mkdir -vp $ERL_TOP \
    && tar -xzf otp-src.tar.gz -C $ERL_TOP --strip-components=1 \
    && rm otp-src.tar.gz \
    && ( cd $ERL_TOP \
    && ./otp_build autoconf \
    && gnuArch="$(dpkg-architecture --query DEB_HOST_GNU_TYPE)" \
    && ./configure --build="$gnuArch" \
    && make -j$(getconf _NPROCESSORS_ONLN) \
    && make install ) \
    && rm -rf $ERL_TOP \
    && find /usr/local -regex '/usr/local/lib/erlang/\(lib/\|erts-\).*/\(man\|doc\|obj\|c_src\|emacs\|info\|examples\)' | xargs rm -rf \
    && find /usr/local -name src | xargs -r find | grep -v '\.hrl$' | xargs rm -v || true \
    && find /usr/local -name src | xargs -r find | xargs rmdir -vp || true \
    && scanelf --nobanner -E ET_EXEC -BF '%F' --recursive /usr/local | xargs -r strip --strip-all \
    && scanelf --nobanner -E ET_DYN -BF '%F' --recursive /usr/local | xargs -r strip --strip-unneeded \
    && runDeps="$( \
    scanelf --needed --nobanner --format '%n#p' --recursive /usr/local \
    | tr ',' '\n' \
    | sort -u \
    | awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
    )" \
    && apk add --virtual .erlang-rundeps \
    $runDeps \
    lksctp-tools \
    ca-certificates \
    && apk del .fetch-deps .build-deps

CMD ["erl"]