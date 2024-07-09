FROM ubuntu:focal

ENV DEBIAN_FRONTEND=noninteractive
RUN echo "APT::Get::Assume-Yes \"true\";" > /etc/apt/apt.conf.d/90assumeyes

# NOTE: When adding more packages, DONT ADD THEM IN THIS SECTION - rather do it further down. That'll be beneficial for layer caching
RUN apt-get -qq update && \
    apt-get -qq install -y build-essential gcc-aarch64-linux-gnu gcc gcc-9-aarch64-linux-gnu \            
            device-tree-compiler make git wget bison flex libghc-libyaml-dev ca-certificates \
            u-boot-tools bc swig libc6-dev libc6-dev-i386 libssl-dev dpkg-dev locales \            
            squashfs-tools libseccomp-dev libcap-dev xfslibs-dev \
            dracut-core sudo adduser xdelta3 qemu-utils linux-image-generic dosfstools libguestfs-tools mtools \
            autoconf autoconf-archive libtool chrpath diffstat zstd tar
            
# Download and extract libseccomp-dev for arm64 to build snapd bins
RUN wget --no-check-certificate http://ports.ubuntu.com/ubuntu-ports/pool/main/libs/libseccomp/libseccomp-dev_2.5.3-2ubuntu2_arm64.deb && dpkg-deb -x libseccomp-dev_2.5.3-2ubuntu2_arm64.deb /

# Install Golang
RUN wget --no-check-certificate https://go.dev/dl/go1.22.0.linux-amd64.tar.gz
RUN tar -C /usr/local -xzf go1.22.0.linux-amd64.tar.gz

# Configure Python
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
#USER builduser
RUN echo "yocto ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Add Python toolchain
RUN apt-get -qq install -y python-is-python3 python3-dev python3-minimal python3-distutils python3-setuptools python3-pip

# Add Linux tools
RUN apt-get -qq install -y vim dos2unix

# Cleanup to make image smaller
RUN rm -rf /var/lib/apt/lists/* > /dev/null

CMD ["bash"]
