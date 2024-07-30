FROM ubuntu:focal

ENV DEBIAN_FRONTEND=noninteractive
RUN echo "APT::Get::Assume-Yes \"true\";" > /etc/apt/apt.conf.d/90assumeyes

# NOTE: When adding more packages, DONT ADD THEM IN THIS SECTION - rather do it further down. That'll be beneficial for layer caching
RUN apt-get -qq update
RUN apt-get -qq install -y build-essential gcc gcc-aarch64-linux-gnu gcc gcc-9-aarch64-linux-gnu gcc-multilib 
RUN apt-get -qq install -y libghc-libyaml-dev libc6-dev libc6-dev-i386 libssl-dev  libseccomp-dev libcap-dev xfslibs-dev  libguestfs-tools liblz4-tool libacl1 libsdl1.2-dev 
RUN apt-get -qq install -y sudo adduser make gawk autoconf autoconf-archive git git-core wget file locales  ca-certificates  zstd tar unzip xz-utils zstd
RUN apt-get -qq install -y squashfs-tools dpkg-dev debianutils xdelta3
RUN apt-get -qq install -y iputils-ping vim dos2unix xterm
RUN apt-get -qq install -y device-tree-compiler bison flex u-boot-tools bc swig
RUN apt-get -qq install -y dracut-core qemu-utils linux-image-generic dosfstools mtools
RUN apt-get -qq install -y libtool chrpath diffstat socat cpio texinfo
RUN apt-get -qq install -y python3 python3-pip python3-pexpect python-is-python3 python3-dev python3-minimal python3-distutils python3-setuptools python3-pip
RUN apt-get -qq install -y python3-git python3-jinja2 python3-subunit 

RUN sudo locale-gen en_US.UTF-8

# Path for manual installs
RUN mkdir /tmp/install && cd /tmp/install

# Download and extract libseccomp-dev for arm64 to build snapd bins
RUN wget --no-check-certificate http://ports.ubuntu.com/ubuntu-ports/pool/main/libs/libseccomp/libseccomp-dev_2.5.3-2ubuntu2_arm64.deb && dpkg-deb -x libseccomp-dev_2.5.3-2ubuntu2_arm64.deb / 

# Install Golang
RUN wget --no-check-certificate https://go.dev/dl/go1.22.0.linux-amd64.tar.gz
RUN tar -C /usr/local -xzf go1.22.0.linux-amd64.tar.gz

# Setup locale
RUN sed -i '/en_US.UTF-8/s/^# //g' /etc/locale.gen && \
    locale-gen
ENV LANG="en_US.UTF-8"
ENV LANGUAGE="en_US:en"
ENV LC_ALL="en_US.UTF-8"

# Adding repo sync for yocto in docker builds
RUN wget https://storage.googleapis.com/git-repo-downloads/repo && chmod a+x repo && mv repo /usr/local/bin/repo
ENV PATH="/usr/local/bin:$PATH" 
ENV PATH=$PATH:/usr/local/go/bin

# Add user to sudo without password
RUN adduser --disabled-password yocto
RUN adduser yocto sudo
#USER builduser
RUN echo "yocto ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Configure Python toolchain
# RUN ln -s /usr/bin/python3 /usr/bin/python
RUN pip install --upgrade pip
RUN pip install build

# Install libraries for determinism.py
RUN pip install base58==2.1.1
RUN pip install pyyaml==6.0.1
RUN pip install colorlog==6.7.0
RUN pip install requests==2.31.0
RUN pip install graphviz==0.20.3

# Install Yocto Build Tools
RUN cd /opt/ && git clone git://git.yoctoproject.org/poky

# Cleanup to make image smaller
RUN rm -rf /var/lib/apt/lists/* > /dev/null

# Cleanup
RUN rm -rf /tmp/install

CMD ["bash"]
