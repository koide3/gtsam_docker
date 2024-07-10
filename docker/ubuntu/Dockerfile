ARG BASE_IMAGE

FROM ${BASE_IMAGE}
ARG GTSAM_TAG

ENV DEBIAN_FRONTEND=noninteractive
ENV LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

ENV NVIDIA_VISIBLE_DEVICES ${NVIDIA_VISIBLE_DEVICES:-all}
ENV NVIDIA_DRIVER_CAPABILITIES ${NVIDIA_DRIVER_CAPABILITIES:+$NVIDIA_DRIVER_CAPABILITIES,}graphics

# Install apt-fast
RUN apt-get update \
  && apt-get install --no-install-recommends -y software-properties-common gnupg ca-certificates \
  && add-apt-repository ppa:apt-fast/stable \
  && apt-get update \
  && apt-get install --no-install-recommends -y apt-fast \
  && apt-fast clean \
  && rm -rf /var/lib/apt/lists/*

RUN echo debconf apt-fast/maxdownloads string 16 | debconf-set-selections && \
  echo debconf apt-fast/dlflag boolean true | debconf-set-selections && \
  echo debconf apt-fast/aptmanager string apt-get | debconf-set-selections

RUN apt-fast update \
  && apt-fast upgrade -y \
  # install dependencies
  && apt-fast install --no-install-recommends -y \
    wget nano build-essential clang lld libomp-dev \
    git cmake libeigen3-dev ca-certificates \
    libtbb-dev libboost-all-dev libgtest-dev libmetis-dev \
    libglm-dev libglfw3-dev libpng-dev libjpeg-dev \
  && apt-fast clean \
  && rm -rf /var/lib/apt/lists/*

WORKDIR /root
RUN git clone https://github.com/borglab/gtsam.git \
  && cd gtsam \
  && git checkout ${GTSAM_TAG} \
  && mkdir build && cd build \
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
