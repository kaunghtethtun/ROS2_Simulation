# Use official ROS 2 Humble base image
FROM ros:humble

# Set the shell to bash
SHELL ["/bin/bash", "-c"]

# Set environment variables
ENV LANG=C.UTF-8 \
    LC_ALL=C.UTF-8 \
    DEBIAN_FRONTEND=noninteractive

# Install necessary dependencies (modify as needed)
RUN apt-get update && apt-get install -y \
    python3-colcon-common-extensions ros-humble-robot-localization \
    python3-pip tmux tmuxinator evince ros-humble-cartographer-rviz \
    build-essential ros-humble-gazebo-ros-pkgs ros-humble-joint-state-publisher \
    git vim ros-humble-desktop-full ros-humble-twist-mux \
    ros-humble-rmw-cyclonedds-cpp ros-humble-rviz-imu-plugin ros-humble-rqt-tf-tree \
    ros-humble-gazebo-ros2-control ros-humble-gazebo-ros2-control-demos \
    ros-humble-ros2-control ros-humble-ros2-control-test-assets \
    ros-humble-ros2-controllers ros-humble-ros2-controllers-test-nodes \
    ros-humble-navigation2 ros-humble-slam-toolbox ros-humble-cartographer-ros \
    ros-humble-behaviortree-cpp-v3 ros-humble-behaviortree-cpp \ 
    ros-humble-rosbridge-suite ros-humble-plotjuggler \
    && rm -rf /var/lib/apt/lists/*

# Set up workspace
WORKDIR /root/ros2_ws
RUN mkdir -p src

# Copy your simulation package (assuming it's in ~/ros2_sim_pkg)
# COPY ./src /root/ros2_ws/src/

# Build the workspace
RUN source /opt/ros/humble/setup.bash && \
    colcon build --symlink-install

# Optional: Install Python packages if needed
RUN pip install --no-cache-dir \
    numpy \
    flask \
    scipy

# Enable audio
RUN usermod -aG audio root

# Source the workspace automatically
RUN echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
RUN echo "source /root/ros2_ws/install/setup.bash" >> ~/.bashrc
RUN echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
# Set entry point (replace with your launch file)
ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]
