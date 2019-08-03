FROM clearlinux_acrn:29090 as kernel-build
ENV ACRN_PROJECT=https://github.com/projectacrn/acrn-hypervisor.git
ENV http_proxy http://lion.sh.intel.com:4097/
ENV https_proxy http://lion.sh.intel.com:4097/
ENV no_proxy .intel.com
ENV all_proxy socks://lion.sh.intel.com:1080/

# Download and verify ACRN
RUN swupd mirror
#RUN \
#	[ -f acrn-hypervisor ] || git clone ${ACRN_PROJECT}
COPY acrn-hypervisor /acrn-hypervisor
WORKDIR /acrn-hypervisor
RUN make BOARD=generic FIRMWARE=sbl

FROM scratch
ENTRYPOINT []
CMD []
WORKDIR /
COPY --from=kernel-build /acrn-hypervisor/build/* /
