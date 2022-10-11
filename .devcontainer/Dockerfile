FROM osrf/ros:noetic-desktop-full


ARG USERNAME=ros
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME 

RUN rm /bin/sh && ln -s /bin/bash /bin/sh

RUN apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME


# Install Git and vim
RUN apt-get update && apt-get -y upgrade && apt-get install -y git apt-utils python-is-python3 vim
# Create an overlay Catkin workspace
RUN source /opt/ros/noetic/setup.bash 

USER $USERNAME
# Set up the work directory and entrypoint
RUN echo 'source /opt/ros/noetic/setup.bash' >> ~/.bashrc
RUN echo 'source /catkin_ws/devel/setup.bash' >> ~/.bashrc
#WORKDIR /home/catkin_ws


