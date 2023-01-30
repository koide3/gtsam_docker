ARG BASE_IMAGE
FROM ${BASE_IMAGE}
ARG ROS_DISTRO
ENV ROS_DISTRO=$ROS_DISTRO

ENV DEBIAN_FRONTEND=noninteractive
ENV LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

# install ROS
RUN apt-fast update \
  && apt-fast install --no-install-recommends -y curl gnupg lsb-release ca-certificates \
  && sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list' \
  && curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add - \
  && apt-fast update \
  && apt-fast install --no-install-recommends -y \
    python3-rosdep \
    ros-${ROS_DISTRO}-ros-base \
  # clean
  && apt-fast clean \
  && rm -rf /var/lib/apt/lists/*

RUN rosdep init && rosdep update

CMD ["bash"]
