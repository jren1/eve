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
RUN ls -l /acrn-hypervisor
WORKDIR /acrn-hypervisor
RUN make BOARD=generic FIRMWARE=sbl
RUN	mkdir -p /out/boot
RUN	mkdir -p /out/usr/bin
RUN	mkdir -p /out/usr/lib/systemd/system
RUN	mkdir -p /out/usr/share/acrn/bios
RUN	mkdir -p /out/usr/share/acrn/samples/generic
RUN	mkdir -p /out/usr/share/acrn/samples/nuc
RUN	mkdir -p /out/usr/lib/acrn
RUN cp /acrn-hypervisor/build/misc/tools/acrntrace /out/usr/bin/
RUN	cp /acrn-hypervisor/build/misc/tools/acrntrace /out/usr/bin/
RUN	cp /acrn-hypervisor/build/misc/tools/acrnlog.service /out/usr/lib/systemd/system/acrnlog.service
RUN	cp /acrn-hypervisor/build/misc//tools/acrnlog /out/usr/bin/
RUN	cp /acrn-hypervisor/build/misc/tools/acrnctl /out/usr/bin/
RUN	cp /acrn-hypervisor/build/misc/tools/acrnd /out/usr/bin/
RUN	cp /acrn-hypervisor/build/misc/tools/libacrn-mngr.a /out/usr/bin/
RUN	cp /acrn-hypervisor/build/misc/tools/acrnd.service /out/usr/lib/systemd/system/acrnd.service;
RUN	cp /acrn-hypervisor/build/hypervisor/acrn.bin /out/usr/lib/acrn
RUN	cp /acrn-hypervisor/build/hypervisor/acrn.32.out /out/usr/lib/acrn
RUN	cp /acrn-hypervisor/build/hypervisor/acrn.32.out /out/boot
RUN	cp /acrn-hypervisor/build/devicemodel/acrn-dm /out/usr/bin/
RUN	cp /acrn-hypervisor/devicemodel/bios/* /out/usr/share/acrn/bios/
RUN	cp /acrn-hypervisor/misc/efi-stub/clearlinux/acrn.conf /out/usr/share/acrn/samples/nuc/acrn.conf
FROM scratch
ENTRYPOINT []
CMD []
COPY --from=kernel-build /out/ /
