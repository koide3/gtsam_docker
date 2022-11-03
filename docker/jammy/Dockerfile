FROM ubuntu:jammy

ENV DEBIAN_FRONTEND=noninteractive
ENV LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

RUN apt-get update \
  && apt-get upgrade -y \
  # install dependencies
  && apt-get install --no-install-recommends -y \
    wget nano build-essential clang lld \
    git cmake libeigen3-dev ca-certificates \
    libtbb-dev libboost-all-dev libgtest-dev libmetis-dev \
    libglm-dev libglfw3-dev libpng-dev libjpeg-dev \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

WORKDIR /root
RUN git clone https://github.com/borglab/gtsam.git \
  && mkdir gtsam/build && cd gtsam/build \
  && cmake .. \
  -DGTSAM_BUILD_EXAMPLES_ALWAYS=OFF \
  -DGTSAM_BUILD_TESTS=OFF \
  -DGTSAM_WITH_TBB=OFF \
  -DGTSAM_BUILD_WITH_MARCH_NATIVE=OFF \
  -DGTSAM_USE_SYSTEM_EIGEN=ON \
  -DGTSAM_USE_SYSTEM_METIS=ON \
  && make -j$(nproc) \
  && make install \
  && rm -rf /root/gtsam

WORKDIR /root
RUN git clone https://github.com/koide3/iridescence.git --recursive \
  && mkdir iridescence/build && cd iridescence/build \
  && cmake .. \
  && make -j$(nproc) \
  && make install \
  && rm -rf /root/iridescence

CMD ["bash"]
