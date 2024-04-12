FROM ghcr.io/void-linux/void-glibc-busybox:20240303R1

COPY cleanup.sh /usr/bin/void-cleanup

ENV OTP_VERSION="26.2.4" \
    # Declare runtime dependencies.
    RUNTIME_DEPS=' \
    libstdc++ \
    libssl3 \
    lksctp-tools \
    ncurses-libs \
    '

LABEL org.opencontainers.image.version=$OTP_VERSION

RUN set -xe \
    && OTP_DOWNLOAD_URL="https://github.com/erlang/otp/archive/OTP-${OTP_VERSION}.tar.gz" \
    && OTP_DOWNLOAD_SHA256="c3bd54ee83282764f3c236e3aa49d681bac0b4011242e3512d66a7ee2ed79819" \
    && fetchDeps=' \
    curl' \
	&& xbps-install -Sy \
	&& xbps-install -yu xbps \
    && xbps-install -Ay $fetchDeps \
    && curl -fSL -o otp-src.tar.gz "$OTP_DOWNLOAD_URL" \
    && echo "$OTP_DOWNLOAD_SHA256  otp-src.tar.gz" | sha256sum -c - \
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
    && xbps-install -Ay $buildDeps \
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
    && xbps-remove -Roy $buildDeps $fetchDeps \
    # Install runtime dependencies (must be done after cleaning build dependencies)
    && xbps-install -y $RUNTIME_DEPS \ 
    && rm -rf otp-src.tar.gz $ERL_TOP \
    && void-cleanup

CMD ["erl"]
