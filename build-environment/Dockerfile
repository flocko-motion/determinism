FROM ubuntu:noble

ENV DEBIAN_FRONTEND=noninteractive
RUN echo "APT::Get::Assume-Yes \"true\";" > /etc/apt/apt.conf.d/90assumeyes

# NOTE: When adding more packages, DONT ADD THEM IN THIS SECTION - rather do it further down. That'll be beneficial for layer caching
RUN apt-get -qq update
RUN apt-get -qq install -y build-essential gcc-aarch64-linux-gnu gcc gcc-9-aarch64-linux-gnu
RUN apt-get -qq install -y gcc-multilib libghc-libyaml-dev libc6-dev libc6-dev-i386 libssl-dev  libseccomp-dev libcap-dev xfslibs-dev  libguestfs-tools liblz4-tool libacl1 libsdl1.2-dev gdisk
RUN apt-get -qq install -y sudo adduser make gawk autoconf autoconf-archive git git-core wget file locales  ca-certificates  zstd tar unzip xz-utils zstd
RUN apt-get -qq install -y squashfs-tools dpkg-dev debianutils xdelta3
RUN apt-get -qq install -y iputils-ping vim nano dos2unix xterm jq
RUN apt-get -qq install -y device-tree-compiler bison flex u-boot-tools bc swig
RUN apt-get -qq install -y dracut-core qemu-utils linux-image-generic dosfstools mtools
RUN apt-get -qq install -y libtool chrpath diffstat socat cpio texinfo
RUN apt-get -qq install -y python3 python3-pip python3-pexpect python3-dev python3-setuptools
RUN apt-get -qq install -y python3-git python3-jinja2 python3-subunit 
RUN apt-get -qq install -y gcc-9-aarch64-linux-gnu gcc-aarch64-linux-gnu rsync
RUN apt-get -qq install -y autoconf automake autotools-dev bash-completion debhelper dh-apparmor dh-golang fakeroot gcc-multilib 
RUN apt-get -qq install -y grub-common gnupg2 libapparmor-dev indent libcap-dev libfuse3-dev libglib2.0-dev liblzma-dev liblzo2-dev libseccomp-dev 
RUN apt-get -qq install -y libudev-dev pkg-config python3-docutils python3-markdown tzdata xfslibs-dev  dbus squashfs-tools udev gcc-10 autoconf-archive flake8 python3-bs4 python3-pytest
RUN apt-get -qq install -y libfuse-dev

RUN sudo locale-gen en_US.UTF-8

# Path for manual installs
RUN mkdir /tmp/install && cd /tmp/install

# Download and extract libseccomp-dev for arm64 to build snapd bins
RUN wget --no-check-certificate http://ports.ubuntu.com/ubuntu-ports/pool/main/libs/libseccomp/libseccomp-dev_2.5.5-1ubuntu6_arm64.deb && dpkg-deb -x libseccomp-dev_2.5.5-1ubuntu6_arm64.deb / 

# Install Golang
RUN wget --no-check-certificate https://go.dev/dl/go1.24.1.linux-amd64.tar.gz
RUN tar -C /usr/local -xzf go1.24.1.linux-amd64.tar.gz

# Setup locale
RUN sed -i '/en_US.UTF-8/s/^# //g' /etc/locale.gen && \
    locale-gen
ENV LANG="en_US.UTF-8"
ENV LANGUAGE="en_US:en"
ENV LC_ALL="en_US.UTF-8"

# Adding repo sync for yocto in docker builds
RUN wget https://storage.googleapis.com/git-repo-downloads/repo && chmod a+x repo && mv repo /usr/local/bin/repo
ENV PATH="/usr/local/bin:$PATH:/root/.local/bin:/home/yocto/.local/bin"
ENV PATH=$PATH:/usr/local/go/bin

# Add user to sudo without password
RUN adduser --disabled-password yocto
RUN adduser yocto sudo
#RUN adduser ubuntu sudo
#USER builduser
RUN echo "yocto ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
RUN echo "ubuntu ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Configure Python toolchain
# RUN ln -s /usr/bin/python3 /usr/bin/python
RUN apt-get -qq install -y python3-build python3-pip

RUN apt-get -qq install -y gcc-aarch64-linux-gnu

# Install yq without pip and use latest from github, in pip is 3.4.3, current is 4.44
RUN wget https://github.com/mikefarah/yq/releases/latest/download/yq_linux_amd64 -O /usr/bin/yq &&\
    chmod +x /usr/bin/yq

# Cleanup to make image smaller
RUN rm -rf /var/lib/apt/lists/* > /dev/null

# Cleanup
RUN rm -rf /tmp/install

CMD ["bash"]

