FROM nvidia/cuda:11.2.2-devel-ubuntu20.04

ENV DEBIAN_FRONTEND=noninteractive
ENV LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

ENV NVIDIA_VISIBLE_DEVICES ${NVIDIA_VISIBLE_DEVICES:-all}
ENV NVIDIA_DRIVER_CAPABILITIES ${NVIDIA_DRIVER_CAPABILITIES:+$NVIDIA_DRIVER_CAPABILITIES,}graphics

RUN apt-get update \
  && apt-get upgrade -y \
  # add gtsam respository
  && apt-get install --no-install-recommends -y \
    software-properties-common \
  && add-apt-repository ppa:borglab/gtsam-develop \
  # install dependencies
  && apt-get install --no-install-recommends -y \
    wget nano build-essential clang lld libomp-dev \
    git cmake libeigen3-dev ca-certificates \
    libtbb-dev libboost-all-dev libgtest-dev libmetis-dev \
    libgtsam-dev libgtsam-unstable-dev \
    libglm-dev libglfw3-dev libpng-dev libjpeg-dev \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

WORKDIR /root
RUN git clone https://github.com/koide3/iridescence.git --recursive \
  && mkdir iridescence/build && cd iridescence/build \
  && cmake .. \
  && make -j$(nproc) \
  && make install \
  && rm -rf /root/iridescence

CMD ["bash"]
