FROM alpine:latest

RUN apk add --no-cache --virtual .build-deps \
         git \
         cmake \
         make \
         g++ \
         musl-dev \
         libuv-dev \
         openssl-dev \
         libmicrohttpd-dev \         
 && mkdir -p /xmrig/build \
 && cd /xmrig \
 && git clone https://github.com/xmrig/xmrig.git \
 && sed -i 's/constexpr const int kDefaultDonateLevel.*/constexpr const int kDefaultDonateLevel = 0;/'  xmrig/src/donate.h \
 && sed -i 's/constexpr const int kMinimumDonateLevel.*/constexpr const int kMinimumDonateLevel = 0;/'  xmrig/src/donate.h \
 && cd build \
 && cmake ../xmrig -DWITH_HWLOC=OFF \
 && make -j$(nproc)  \
 && ./xmrig --version \
 && cp ./xmrig /usr/local/bin \
 && apk del .build-deps \
 && cd / \
 && rm -rf /xmrig \
 && apk add --no-cache \
         libuv \
         libmicrohttpd \
         util-linux \
		 bash

COPY docker-entrypoint.sh /usr/local/bin/

RUN chmod +x /usr/local/bin/docker-entrypoint.sh

ENTRYPOINT ["docker-entrypoint.sh"]
