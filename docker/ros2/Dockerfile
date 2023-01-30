ARG BASE_IMAGE
FROM ${BASE_IMAGE}
ARG ROS_DISTRO
ENV ROS_DISTRO=$ROS_DISTRO

ENV DEBIAN_FRONTEND=noninteractive
ENV LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

# install ROS
RUN apt-fast update \
  && apt-fast install --no-install-recommends -y curl \
  && curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg \
  && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null \
  && apt-fast update \
  && apt-fast install --no-install-recommends -y \
    python3-colcon-common-extensions \
    python3-rosdep \
    ros-${ROS_DISTRO}-ros-base \
  # clean
  && apt-fast clean \
  && rm -rf /var/lib/apt/lists/*

RUN rosdep init && rosdep update

CMD ["bash"]
