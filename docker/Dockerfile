FROM ros:humble

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    python3-colcon-common-extensions \
    python3-rosdep \
    python3-vcstool \
    git \
    vim \
    curl \
    wget \
    build-essential \
    gdb \
    net-tools \
    iputils-ping \
    mesa-utils \
    x11-apps \
    ros-humble-rviz2 \
    ros-humble-demo-nodes-cpp \
    ros-humble-turtlesim \
    && rm -rf /var/lib/apt/lists/*

RUN rosdep init || true
RUN rosdep update

WORKDIR /workspace

RUN echo "source /opt/ros/humble/setup.bash" >> /root/.bashrc