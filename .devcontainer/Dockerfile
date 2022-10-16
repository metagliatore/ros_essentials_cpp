FROM nvidia/cudagl:11.4.2-base-ubuntu20.04

ARG USERNAME=ros
ARG USER_UID=1000
ARG USER_GID=$USER_UID



RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME 

RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# Minimal setup
RUN apt-get update && apt-get -y upgrade \
    && apt-get install -y locales lsb-release \
    && apt-get install -y sudo \
    && apt-get install -y apt-utils git python-is-python3 vim \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

ARG DEBIAN_FRONTEND=noninteractive
RUN dpkg-reconfigure locales


# Install ROS Noetic
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
RUN apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
RUN apt-get update \
 && apt-get install -y build-essential \
 && apt-get install -y --no-install-recommends ros-noetic-desktop-full
RUN apt-get install -y --no-install-recommends python3-rosdep
RUN rosdep init \
 && rosdep fix-permissions \
 && rosdep update
RUN echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
RUN echo "source /opt/ros/noetic/setup.bash" >> /home/$USERNAME/.bashrc


##---------------turtlebot3 installation----------------------------
#FROM nvidia_ros:latest

# Change the default shell to Bash
SHELL [ "/bin/bash" , "-c" ]

# Create a Catkin workspace and clone TurtleBot3 repos
RUN source /opt/ros/noetic/setup.bash \
 && mkdir -p /turtlebot3_ws/src \
 && cd /turtlebot3_ws/src \
 && catkin_init_workspace \
 && git clone -b noetic-devel https://github.com/ROBOTIS-GIT/turtlebot3.git \
 && git clone -b noetic-devel https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git \
 && git clone -b noetic-devel https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
RUN echo "export TURTLEBOT3_MODEL=waffle_pi" >> /home/$USERNAME/.bashrc
RUN echo "export TURTLEBOT3_MODEL=waffle_pi" >> ~/.bashrc

RUN apt-get install -y ros-noetic-usb-cam ros-noetic-image-view ros-noetic-vision-opencv

# Build the Catkin workspace and ensure it's sourced
RUN source /opt/ros/noetic/setup.bash \
 && cd /turtlebot3_ws \
 && catkin_make
RUN echo "source /turtlebot3_ws/devel/setup.bash" >> /home/$USERNAME/.bashrc
RUN echo "source /turtlebot3_ws/devel/setup.bash" >> ~/.bashrc


USER $USERNAME


RUN echo 'source /catkin_ws/devel/setup.bash' >> ~/.bashrc
#WORKDIR /home/catkin_ws