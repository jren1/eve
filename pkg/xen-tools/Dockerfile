FROM alpine:3.8 as kernel-build
RUN apk add --no-cache \
    	    gcc make libc-dev dev86 xz-dev perl bash python-dev \
	    gettext iasl util-linux-dev ncurses-dev glib-dev \
	    pixman-dev libaio-dev yajl-dev argp-standalone \
	    linux-headers git patch texinfo curl tar bash socat openssh

ENV http_proxy http://lion.sh.intel.com:4097/
ENV https_proxy http://lion.sh.intel.com:4097/
ENV no_proxy .intel.com
ENV all_proxy socks://lion.sh.intel.com:1080/
ENV GIT_PROXY_COMMAND /oe-git-proxy
# FIXME: this is really ugly -- we need to figure out xen tools dependencies
RUN [ `uname -m` = "aarch64" ] && apk add --no-cache dtc-dev || :

# Alpine linux defines all 64bit integer types as long. Patch
# /usr/include/bits/alltypes.h to fix compilation with -m32
WORKDIR /
COPY alpine.patch /
COPY oe-git-proxy /
RUN patch -p1 < alpine.patch

ENV XEN_VERSION 4.12.0
ENV XEN_SOURCE=https://downloads.xenproject.org/release/xen/${XEN_VERSION}/xen-${XEN_VERSION}.tar.gz

# Download and verify xen
#TODO: verify Xen
RUN \
    [ -f xen-${XEN_VERSION}.tar.gz ] || curl --proxy-insecure --insecure -fsSLO ${XEN_SOURCE} && \
    gzip -d xen-${XEN_VERSION}.tar.gz && \
    cat xen-${XEN_VERSION}.tar | tar --absolute-names -x && mv /xen-${XEN_VERSION} /xen

# Apply local patches
COPY patches-${XEN_VERSION} /patches
WORKDIR /xen
RUN set -e && for patch in /patches/*.patch; do \
        echo "Applying $patch"; \
        patch -p1 < "$patch"; \
    done

RUN mkdir -p /out

WORKDIR /xen
RUN ./configure --prefix=/usr --disable-xen --disable-qemu-traditional --disable-docs --disable-stubdom
RUN make && make dist
RUN dist/install.sh /out

FROM scratch
ENTRYPOINT []
CMD []
COPY --from=kernel-build /out/ /
