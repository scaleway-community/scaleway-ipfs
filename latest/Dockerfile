## -*- docker-image-name: "scaleway/ipfs:latest" -*-
ARG SCW_ARCH
ARG MULTIARCH_ARCH

FROM scaleway/ubuntu:${SCW_ARCH}-bionic

MAINTAINER Scaleway <opensource@scaleway.com> (@scaleway)

# Prepare rootfs for image-builder
RUN /usr/local/sbin/builder-enter

ENV ARCH "amd64"

# Downloads IPFS and install it
RUN wget https://dist.ipfs.io/$(curl -s https://dist.ipfs.io/#go-ipfs \
          | grep -oP "<a href=\"\Kgo-ipfs.*linux-$ARCH.*(?=\">)") \
         -O go-ipfs.tar.gz \
    && tar xzvf go-ipfs.tar.gz \
    && rm go-ipfs.tar.gz \
    && cd go-ipfs \
    && ./install.sh \
    && ipfs init

# Copy files
COPY overlay ./

# Create server specific doc in /root/ for first login
RUN systemctl enable create-doc ipfs

# Symlink doc readme to /root/
RUN ln -s /usr/share/doc/scaleway/ipfs/README /root/README

# Clean rootfs from image-builder
RUN /usr/local/sbin/builder-leave
