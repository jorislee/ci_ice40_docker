# ice40 compilation environment docker

Based on python:3.7.7 configure ice40 compilation environment docker.

## Dockerfile
  ``` shell
  #python:3.7.7, Debian GNU/Linux 10
  FROM python:3.7.7

  ARG DEBIAN_FRONTEND=noninteractive
      
  RUN apt-get update && apt-get install -y \
    # tools
    libusb-1.0-0 \
    usbutils \
    libftdi1 \
    busybox \
    vim \
    # icestom arachne-pnr nextpnr yosos
    build-essential \
    clang \
    bison \
    flex \
    libreadline-dev \
    gawk \
    tcl-dev \
    libffi-dev \
    git \
    mercurial \
    graphviz \
    xdot \
    pkg-config \
    python \
    libftdi-dev \
    python3-dev \
    libboost-all-dev \
    cmake \
    wget \
    python3 \
    python3-setuptools \
    nano \
    cmake \
    libeigen3-dev \
    gperf \
    && rm -rf /var/lib/apt/lists/* \
    # icestorm
    && git clone --recursive https://github.com/cliffordwolf/icestorm.git icestorm \
    && cd icestorm && make clean && make -j$(nproc) && make install && cd - && rm -r icestorm \
    # yosys
    && git clone --recursive https://github.com/cliffordwolf/yosys.git yosys \
    && cd yosys && make clean && make yosys-abc \
    && make -j$(nproc) && make install && cd - && rm -r yosys \
    # libseccomp
    && git clone --recursive https://github.com/seccomp/libseccomp.git libseccomp \
    && cd libseccomp && ./autogen.sh && ./configure &&  make [V=0] \
    && make install && cd - && rm -r libseccomp \
    # arachne-pnr
    && git clone --recursive https://github.com/cseed/arachne-pnr.git arachne-pnr \
    && cd arachne-pnr && make clean && make -j$(nproc) && make install && cd - && rm -r arachne-pnr \
    # nextpnr
    && git clone --recursive https://github.com/YosysHQ/nextpnr nextpnr \
    && cd nextpnr && cmake -DARCH=ice40 -DBUILD_GUI=OFF -DCMAKE_INSTALL_PREFIX=/usr/local . \
    && make -j$(nproc) && make clean && make install && cd - && rm -r nextpnr \
    # iverilog
    && git clone --recursive https://github.com/steveicarus/iverilog.git iverilog \
    && cd iverilog && autoconf && ./configure && make clean \
    && make -j$(nproc) && make install && cd - && rm -r iverilog \
    # verilator
    && git clone --recursive https://github.com/ddm/verilator.git verilator \
    && cd verilator && autoconf && ./configure && make clean \
    && make -j$(nproc) && make install && cd - && rm -r verilator
 
  CMD [ "/bin/bash" ]
  ```

## License

MIT License.

## Acknowledgments

* [icestorm-docker](https://github.com/cranphin/icestorm-docker)
* Stackoverflow

