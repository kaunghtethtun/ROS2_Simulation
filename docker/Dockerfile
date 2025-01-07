# Use the official ROS 2 Humble base image from Docker Hub
FROM ros:humble

# Set environment variables
ENV LANG=C.UTF-8 \
    LC_ALL=C.UTF-8 \
    DEBIAN_FRONTEND=noninteractive

# Install additional dependencies
RUN apt-get update && \
    apt-get install -y \
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


# Create a ROS 2 workspace directory
RUN mkdir -p /test_ws/src

# Set the working directory to the ROS 2 workspace
RUN mkdir -p /bobo_ws/src
RUN mkdir -p /ros2_ws/src
WORKDIR /ros2_ws

# Build the workspace (uncomment if you have packages to build)
RUN . /opt/ros/humble/setup.sh && colcon build

# Source ROS 2 setup script
RUN echo 'source /opt/ros/humble/setup.bash' >> ~/.bashrc
RUN echo '#export ROM_ROBOT_MODEL=four_wheel' >> ~/.bashrc
RUN echo '#export ROM_ROBOT_MODEL=drone' >> ~/.bashrc
RUN echo 'export ROM_ROBOT_MODEL=bobo' >> ~/.bashrc
RUN echo 'alias bb="colcon build --symlink-install && source install/setup.bash"' >> ~/.bashrc
RUN echo 'alias delete_workspace="rm -rf build install log;"' >> ~/.bashrc
RUN echo '#export ROM_GZ_WORLD=square.world' >> ~/.bashrc
RUN echo '#export ROM_GZ_WORLD=willow.world' >> ~/.bashrc
RUN echo '#export ROM_GZ_WORLD=sonoma_raceway.world' >> ~/.bashrc
RUN echo '#export ROM_GZ_WORLD=maze.world' >> ~/.bashrc
RUN echo 'source /usr/share/gazebo/setup.bash' >> ~/.bashrc
RUN echo 'export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp' >> ~/.bashrc 
RUN echo 'export ROS_DOMAIN_ID=69' >> ~/.bashrc 
## copy ~/.gazebo/models /root/.gazebo/models----------------------------------------------------------

# Optional: Install Python packages if needed
RUN pip install --no-cache-dir \
    numpy \
    flask \
    scipy

# Copy local ROS 2 packages into the workspace (if any)
#COPY ../bobo* /ros2_ws/src/
# COPY .tmux.conf /root/.tmux.conf
# COPY .tmuxinator.yml /rom_ws/.tmuxinator.yml

# Source ROS 2 setup script
RUN echo '#source /ros2_ws/install/setup.bash' >> ~/.bashrc
RUN echo '#source /test_ws/install/setup.bash' >> ~/.bashrc
RUN echo '#source /bobo_ws/install/setup.bash' >> ~/.bashrc

# Set entrypoint to the ROS 2 bash shell
ENTRYPOINT ["/ros_entrypoint.sh"]

CMD ["bash"]
