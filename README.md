# ice40 compilation environment docker

Based on python:3.7.7 configure ice40 compilation environment docker.

## Dockerfile
  ``` shell
  FROM ubuntu:20.04

  ARG DEBIAN_FRONTEND=noninteractive

  RUN apt-get update && apt-get install -y \
      bison \
      build-essential \
      clang \
      cmake \
      flex \
      gawk \
      git \
      graphviz \
      libboost-all-dev \
      libeigen3-dev \
      libffi-dev \
      libftdi-dev \
      libreadline-dev \
      mercurial \
      pkg-config \
      python3 \
      python3-dev \
      tcl-dev \
      xdot \
      autoconf \
      bison \
      flex \
      g++ \
      gcc \
      git \
      gperf \
      gtkwave \
      make \
      libhidapi-dev \
      libusb-1.0-0 \
      libusb-1.0-0-dev \
      zlib1g-dev \
      libevent-dev \
      libjson-c-dev \
      verilator \
      && rm -rf /var/lib/apt/lists/*

  # yosys
  RUN git clone --recursive https://github.com/cliffordwolf/yosys.git yosys \
      && cd yosys && make clean && make config-clang \
      && make -j$(nproc) && make install && cd - && rm -r yosys

  RUN git clone --recursive https://github.com/YosysHQ/icestorm.git icestorm \
      && cd icestorm && make clean && make make -j$(nproc) \
      && make install && cd - && rm -r icestorm

  # nextpnr
  RUN git clone --recursive https://github.com/YosysHQ/nextpnr.git nextpnr \
      && cd nextpnr && cmake -DARCH=ice40 -DTRELLIS_INSTALL_PREFIX=/usr/local . \
      && make -j$(nproc) && make install && cd - && rm -r nextpnr

  # iverilog
  RUN git clone --recursive https://github.com/steveicarus/iverilog.git iverilog \
      && cd iverilog && sh autoconf.sh && ./configure \
      && make -j$(nproc) && make install && cd - && rm -r iverilog

  CMD [ "/bin/bash" ]
 
  ```

## License

MIT License.

## Acknowledgments

* Stackoverflow

