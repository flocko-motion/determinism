FROM ubuntu:focal

ENV DEBIAN_FRONTEND=noninteractive
RUN echo "APT::Get::Assume-Yes \"true\";" > /etc/apt/apt.conf.d/90assumeyes
RUN apt-get -qq update && \
    apt-get -qq install -y build-essential gcc-aarch64-linux-gnu gcc gcc-9-aarch64-linux-gnu \
            device-tree-compiler make git wget bison flex libghc-libyaml-dev ca-certificates \
            python3-minimal python3-distutils u-boot-tools bc swig python3-dev libc6-dev libc6-dev-i386 \
            libssl-dev dpkg-dev locales python3-setuptools squashfs-tools libseccomp-dev libcap-dev xfslibs-dev \
            dracut-core sudo adduser xdelta3 sudo qemu-utils linux-image-generic dosfstools libguestfs-tools mtools sudo autoconf autoconf-archive libtool make && rm -rf /var/lib/apt/lists/* > /dev/null
# Download and extract libseccomp-dev for arm64 to build snapd bins
RUN wget --no-check-certificate http://ports.ubuntu.com/ubuntu-ports/pool/main/libs/libseccomp/libseccomp-dev_2.5.3-2ubuntu2_arm64.deb && dpkg-deb -x libseccomp-dev_2.5.3-2ubuntu2_arm64.deb /
# Install Golang
RUN apt-get -qq update && \
    apt-get -qq install -y chrpath diffstat zstd tar
# chrpath diffstat pzstd unzstd zstd
RUN wget --no-check-certificate https://go.dev/dl/go1.22.0.linux-amd64.tar.gz
RUN tar -C /usr/local -xzf go1.22.0.linux-amd64.tar.gz
RUN ln -s /usr/bin/python3 /usr/bin/python
RUN sed -i '/en_US.UTF-8/s/^# //g' /etc/locale.gen && \
    locale-gen
ENV LANG en_US.UTF-8  
ENV LANGUAGE en_US:en  
ENV LC_ALL en_US.UTF-8
# Adding repo sync for yocto in docker builds
RUN wget https://storage.googleapis.com/git-repo-downloads/repo && chmod a+x repo && mv repo /usr/local/bin/repo
ENV PATH="/usr/local/bin:$PATH" 
ENV PATH=$PATH:/usr/local/go/bin
# Add user to sudo without password
RUN adduser --disabled-password yocto
RUN adduser yocto sudo
RUN echo "yocto ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
RUN apt-get -qq update && \
apt-get -qq install -y python-is-python3
#USER builduser


CMD ["bash"]
