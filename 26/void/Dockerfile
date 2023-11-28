FROM ghcr.io/void-linux/void-glibc-busybox:20231003R1

COPY cleanup.sh /usr/bin/void-cleanup

ENV OTP_VERSION="26.1.2"

LABEL org.opencontainers.image.version=$OTP_VERSION

RUN set -xe \
    && OTP_DOWNLOAD_URL="https://github.com/erlang/otp/archive/OTP-${OTP_VERSION}.tar.gz" \
    && OTP_DOWNLOAD_SHA256="56042d53b30863d4e720ebf463d777f0502f8c986957fc3a9e63dae870bbafe0" \
    && fetchDeps=' \
    curl' \
    && xbps-install -Suy \
    && xbps-install -y $fetchDeps \
    && curl -fSL -o otp-src.tar.gz "$OTP_DOWNLOAD_URL" \
    && echo "$OTP_DOWNLOAD_SHA256  otp-src.tar.gz" | sha256sum -c - \
    && runtimeDeps=' \
    libssl3 \
    lksctp-tools \
    ' \
    && buildDeps=' \
    autoconf \
    dpkg \
    gcc \
    make \
    ncurses-devel \
    openssl-devel \
    lksctp-tools-devel \
    pax-utils \
    binutils \
    ' \
    && xbps-install -y $buildDeps \
    && export ERL_TOP="/usr/src/otp_src_${OTP_VERSION%%@*}" \
    && mkdir -vp $ERL_TOP \
    && tar -xzf otp-src.tar.gz -C $ERL_TOP --strip-components=1 \
    && ( cd $ERL_TOP \
    && ./otp_build autoconf \
    && gnuArch="$(dpkg-architecture --query DEB_HOST_GNU_TYPE)" \
    && ./configure --build="$gnuArch" \
    && make -j$(getconf _NPROCESSORS_ONLN) \
    && make install ) \
    # Clean up
    && find /usr/local -regex '/usr/local/lib/erlang/\(lib/\|erts-\).*/\(man\|doc\|obj\|c_src\|emacs\|info\|examples\)' | xargs rm -rf \
    && find /usr/local -name src | xargs -r find | grep -v '\.hrl$' | xargs rm -v || true \
    && find /usr/local -name src | xargs -r find | xargs rmdir -vp || true \
    && scanelf --nobanner -E ET_EXEC -BF '%F' --recursive /usr/local | xargs -r strip --strip-all \
    && scanelf --nobanner -E ET_DYN -BF '%F' --recursive /usr/local | xargs -r strip --strip-unneeded \
    && xbps-remove -ROoy $buildDeps $fetchDeps \
    # Install runtime dependencies in the back door to avoid being removed by association.
    && xbps-install -y $runtimeDeps \ 
    && rm -rf otp-src.tar.gz $ERL_TOP \
    && void-cleanup

CMD ["erl"]