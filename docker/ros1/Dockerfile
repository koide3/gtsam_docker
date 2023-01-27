ARG BASE_IMAGE=koide3/gtsam_docker:focal
FROM ${BASE_IMAGE}
ARG ROS_DISTRO=noetic

RUN echo ROS_DISTRO=$ROS_DISTRO

ENV DEBIAN_FRONTEND=noninteractive
ENV LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

# install ROS
RUN apt-get update \
  && apt-get install --no-install-recommends -y \
    curl gpg-agent \
  && sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list' \
  && curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add - \
  && apt-get update \
  && apt-get install --no-install-recommends -y \
    python3-rosdep \
    ros-${ROS_DISTRO}-ros-base \
  # clean
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

RUN rosdep init && rosdep update

CMD ["bash"]
