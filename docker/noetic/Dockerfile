FROM koide3/gtsam_docker:focal

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
    ros-noetic-ros-base \
  # clean
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

RUN rosdep init && rosdep update

CMD ["bash"]
