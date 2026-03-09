# COMP0244 Labs

- [0. Environment Setup & Prerequisites](#0-environment-setup--prerequisites)
- [Lab 1: Robot Teleop & Simulation Basics](#lab-1-robot-teleop--simulation-basics)
- [Lab 2: Autonomous Deployment Challenge](#lab-2-autonomous-deployment-challenge)

---

# 0. Environment Setup & Prerequisites

This repository provides an environment that can be run using Docker. The environment is designed to run specific software or tasks, and the following instructions will guide you through installing dependencies, setting up the Docker container, and running the necessary files.

## Setup your SSH Key in your Github
##### Step 0: Generate a new SSH key. Open a terminal and run:
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

For Mac you need also to do:
```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519                    
```

Then, add the SSH key to your GitHub account (New SSH key):
```bash
cat ~/.ssh/id_rsa.pub
```

<details>
<summary><h2>Installation on Ubuntu</h2></summary>

##### Step 1: Open a terminal and clone the repo:
```bash
mkdir /home/$USER/comp0244_ws
cd /home/$USER/comp0244_ws
git clone --recursive git@github.com:COMP0244-S25/comp0244-go2.git
```

#### Environment: ROS2-Humble
##### Step 2: In the same terminal, download the Docker (https://docs.docker.com/engine/install):
```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

##### Step 3: In the same terminal, download the docker images (click [here](https://aws.amazon.com/docker) to understand docker):
```bash
sudo docker pull jjiao/comp0244:unitree-go-ros2-humble
sudo docker tag jjiao/comp0244:unitree-go-ros2-humble comp0244:unitree-go-ros2-humble
```

##### Step 4: In the same terminal, enable the display:
```bash
sudo apt-get install x11-xserver-utils
xhost +
```
Note that everytime you restart your computer you will probably need to run the xhost + command above.

##### Step 5: In the same terminal, create the docker container:
```bash
sudo docker run -it -e DISPLAY -e QT_X11_NO_MITSHM=1 -e XAUTHORITY=/tmp/.docker.xauth \
-v /home/$USER/comp0244_ws:/usr/app/comp0244_ws \
--network host \
--name comp0244_unitree comp0244:unitree-go-ros2-humble /bin/bash
```

##### Step 6: Exit the docker and start your docker environment
```bash
sudo docker container start comp0244_unitree
sudo docker exec -it comp0244_unitree /bin/bash
```

</details>

<details>
<summary><h2>Installation on Windows</h2></summary>

##### Step 1: Open a terminal (wsl) and clone the repo:
```bash
mkdir /home/$USER/comp0244_ws
cd /home/$USER/comp0244_ws
git clone --recursive git@github.com:COMP0244-S25/comp0244-go2.git
```

#### Environment: ROS2-Humble
##### Step 2: In the same terminal, download the Docker (https://docs.docker.com/engine/install):
```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

##### Step 3: In the same terminal, download the docker images (click [here](https://aws.amazon.com/docker) to understand docker):
```bash
sudo docker pull jjiao/comp0244:unitree-go-ros2-humble
sudo docker tag jjiao/comp0244:unitree-go-ros2-humble comp0244:unitree-go-ros2-humble
```

##### Step 4: Configure WSLg (Required for Windows 11 WSL2 users):
```bash
echo '# WSLg Configuration
export DISPLAY=:0
export WAYLAND_DISPLAY=wayland-0
export XDG_RUNTIME_DIR=/run/user/1000
export PULSE_SERVER=/run/user/1000/pulse/native' >> ~/.bashrc

source ~/.bashrc
```

Verify display setup:
```bash
# Install x11-apps for testing
apt-get update && apt-get install -y x11-apps

# Test the display (should open a window with moving eyes)
xeyes

# If the test is successful, you can proceed with the tutorial
# Press Ctrl+C to close xeyes
```

##### Step 5: In the same terminal, create the docker container:
```bash
sudo docker run -it \
-e DISPLAY=$DISPLAY \
-e WAYLAND_DISPLAY=$WAYLAND_DISPLAY \
-e XDG_RUNTIME_DIR=$XDG_RUNTIME_DIR \
-e PULSE_SERVER=$PULSE_SERVER \
-v /tmp/.X11-unix:/tmp/.X11-unix \
-v /mnt/wslg:/mnt/wslg \
-v /usr/lib/wsl:/usr/lib/wsl \
-v /home/$USER/comp0244_ws:/usr/app/comp0244_ws \
--network host \
--name comp0244_unitree comp0244:unitree-go-ros2-humble /bin/bash
```

##### Step 6: Exit the docker and start your docker environment
```bash
sudo docker container start comp0244_unitree
sudo docker exec -it comp0244_unitree /bin/bash
```

</details>

<details>
<summary><h2>Installation on AppleSilicon</h2></summary>


This repository provides an environment that can be run within an virtual environemnt of ARM architecture. The environment is designed to run specific software or tasks, and the following instructions will guide you through installing dependencies, setting up the Docker container, and running the necessary files.

---

##### Step 1. Install UTM Virtual Machine
1. Visit the [UTM Virtual Machine Website](https://mac.getutm.app/).
2. Download the **free version** of the UTM application (not the App Store version to avoid charges).
3. Open the downloaded `UTM.dmg` file and follow the installation steps.
4. Download the 64-bit [ARM Ubuntu 22.04 image](https://cdimage.ubuntu.com/releases/jammy/release/)

##### Step 2. Set up the Ubunutu 22.04 VM: 
1. Open UTM and click **+** → **Virtualize** → **Linux**.
2. Allocate:
   - **Memory**: 2+ GB.
   - **CPU Cores**: 2+.
3. Add:
   - **Primary Disk**: 30+ GB.
   - **CD/DVD Drive**: Attach the downloaded Ubuntu ISO.
4. Set the **CD/DVD Drive** as the first boot device under **System**.
5. Save and click **Play** to boot the VM into the Ubuntu installer.
6. Follow the installer prompts:
   - Use the entire disk for installation.
   - Set a username, password, and hostname.
   - Enable install of **OpenSSH**
7. Stop the VM.
8. Go to **Drives** → Remove the **CD/DVD Drive**.
9. Save and reboot.

---
##### Step 3. Install Graphics on your VM

1. Update your VM:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. Install the graphic interface:
    ```bash
    sudo apt install -y xubuntu-desktop
    ```
3. When prompted - selected **lightdm**
4. Reboot your environemnt:
    ```bash
    sudo reboot
    ```
---
##### Step 4. Set up your SSH key. 
1.Set SSH key compatible with yout Github account - replace "youremail"
  ```bash
   ssh-keygen -t rsa -b 4096 -C “youremail”@youremail.com
  eval "$(ssh-agent -s)"
  ssh-add ~/.ssh/id_rsa
  cat ~/.ssh/id_rsa.pub
  ```
2. Copy to GitHub ssh keys:
   - In your Github acount go to settings and SSH and GPG Keys.
   - Add a new SSH key and name it "UbunutuVMkey"
   - Coppy the output of the last command.
   - sSave entry 
   
3. Varify correct setup:
   ```bash
   ssh -T git@github.com
    ```
  You should see a "Hey, [your Github username]"
  
##### Step 5. Install ROS2 humble on your VM
1: Setup Locale
Make sure your system locale is set to UTF-8:

```bash
sudo apt update && sudo apt install -y locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
```

---

2: Add the ROS 2 Repository

```bash
sudo apt update && sudo apt install -y software-properties-common
sudo add-apt-repository universe
sudo apt update && sudo apt install -y curl
curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key | sudo apt-key add -
```

Add the ROS 2 repository to your sources list:

```bash
sudo sh -c 'echo "deb [arch=arm64] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list'
```

---

3: Install ROS 2 Humble
Update your package index and install ROS 2:

```bash
sudo apt update
sudo apt install -y ros-humble-desktop
```

For a smaller installation (if you're limited on storage or just need core features):

```bash
sudo apt install -y ros-humble-ros-base
```

---

4: Environment Setup
Source the ROS 2 setup file automatically on terminal startup:

```bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

---

5: Install Additional Tools (Optional)
Install `colcon` for building ROS 2 packages:

```bash
sudo apt install -y python3-colcon-common-extensions
```

Install ROS 2 CLI tools:

```bash
sudo apt install -y python3-argcomplete
```

---

##### Step 6. Install Module Repository: 
Open a terminal and run: 
```bash
mkdir /home/$USER/comp0244_ws
cd /home/$USER/comp0244_ws
git clone --recursive git@github.com:COMP0244-S25/comp0244-go2.git
```
---

##### Step 7. Test the Virtual Machine
1. Open a terminal (Terminator) in the virtual machine, or press `Ctrl+Alt+T`.
2. Run the following command to start Gazebo:
   ```bash
   gazebo
   ```
3. If it launches successfully you are ready to go!
4. Navigate to the specified directory for the course environment:
   ```bash
   cd comp0244/tutorial_env_go2
   ```
5. Source the setup file:
   ```bash
   source install/setup.bash
   ```

</details>

<details>
<summary><h2>Attaching to a Running Docker Container in VS Code</h2></summary>

This would allow access to file explorer. 

### Prerequisites
Before you start, ensure the following are set up:

1. **Docker Installed**: Docker must be installed and running on your machine.
2. **Visual Studio Code Installed**: Make sure you have VS Code installed.
3. **Docker Extension**:
   - Open the Extensions view in VS Code (`Ctrl+Shift+X` or `Cmd+Shift+X` on macOS).
   - Search for "Docker" and install the extension by Microsoft.

### 1. Open the Docker View
- Click on the **Docker** icon in the Activity Bar on the left-hand side of VS Code. If this does not exist you can install the Docker Extension from the VScode extensions. 

### 2. Locate Your Running Container
- Under the **Containers** section, find the container you want to attach to.
- Running containers will have a green status indicator.

### 3. Attach to the Container
- Right-click on the container name and choose one of the following options:
  - **Attach Shell**: Opens a terminal session attached to the container.
  - **Attach Visual Studio Code**: Opens the container's filesystem as a remote workspace in VS Code.

### 4. Explore Files and Debug
- If you selected **Attach Visual Studio Code**, the container will open as a remote workspace. You can now:
  - Browse and edit files within the container.
  - Use the integrated terminal to run commands in the container.

### Additional Tips

### Restarting a Stopped Container
If the container is stopped, you can restart it:
- Right-click on the container in the Docker view and select **Start**.

### Using the Command Palette
You can also use the Command Palette:
- Open the Command Palette (`Ctrl+Shift+P` or `Cmd+Shift+P` on macOS).
- Type and select `Docker: Attach Shell` or `Remote-Containers: Attach to Running Container`.

</details>

---

# Lab 1: Robot Teleop & Simulation Basics
**Goal:** In this lab, student groups will alternate between operating the real unitree robot and completing exercises in the simulation. This rotation ensures that everyone gets hands-on experience safely while working on core robotic tasks.

<details>
<summary><h2>Real Robot Station (Alternating by groups)</h2></summary>

**Safety First!**
1. Read carefully the instructions shared in moodle named: OPS-G07-SOP-Go2
2. Make sure you understand how to insert a battery and how to turn the robot on/off.
3. Make sure you understand how to use the two external controllers to move the robot, to stop it, and put it down.
4. Make sure you need at least 3 people operating the robot: one controlling via the terminal, the other having the two controllers, and one on the crane.
5. Do not put at any case fingers/jewleries/hair close to robot motors.
6. Long and loose hair must be contained, Rings and jewellery must not be worn.Budges or any other items risking being trapped in the motors must not be worn.
7. Teams will be operating one by one.

### Task 1: Real Robot Cmd Velocity
To operate the robot we will use two computers:

#### Team 1: UCL Operator PC3, GO2 No.1, and Student's Computer
##### 1. Turn on the GO2 No.1
Install the battery, double-click the batter switch, and wait for about 30s to make sure the robot to be started

##### 2. UCL Operator PC3: 
###### 2.1. Connect to the wifi Arvin with the password (see the note on the screen).
###### 2.2. Open a new terminal:
```bash
source ~/opt/ros/foxy/setup.bash
source ~/Documents/ros2_ws/install/setup.bash
export ROBOT_IP="172.20.10.6"
export CONN_TYPE="webrtc"
ping 172.20.10.6
```
```bash
ros2 launch go2_robot_sdk test.launch.py
```

##### 3. Student's computer:
###### 3.1. Connect to the wifi Arvin with the password (see the note on the screen).
###### 3.2. Open the docker environment:
```bash
sudo docker container start comp0244_unitree
sudo docker exec -it comp0244_unitree /bin/bash
source /opt/ros/humble/setup.bash
export ROBOT_IP="172.20.10.6"
export CONN_TYPE="webrtc"
ping 172.20.10.6
```
###### 3.3 Publish the velocity on the topic /cmd_vel to have the robot walk. 
NOTE: not exceed 0.4m/s for linear velocity.
```bash
ros2 topic pub /cmd_vel -r 10 geometry_msgs/msg/Twist '{
  linear: {x: 0.2, y: 0.0, z: 0.0},
  angular: {x: 0.0, y: 0.0, z: 0.0}
}'
```
###### 3.4 IMPORTANT NOTE:
```bash
Do not forget to DISCONNECT the wifi once you finish the task
```

#### Team 2: UCL Operator PC2, GO2 No.2, and Student's Computer
##### 1. Start the GO2 No.2:
Install the battery, double-click the batter switch, and wait for about 30s to make sure the robot to be started

##### 2. UCL Operator PC2: 
###### 2.1. Connect to the wifi jjiaoiPhone with the password (see the note on the screen).
###### 2.2. Open a new terminal:
```bash
conda deactivate
source ~/Documents/ros2_ws/install/setup.bash
export ROBOT_IP="172.20.10.3"
export CONN_TYPE="webrtc"
ping 172.20.10.3
```
```bash
ros2 launch go2_robot_sdk test.launch.py
```

##### 3. Student's computer:
###### 3.1 Connect to the wifi jiaoiPhone with the password (see the note on the screen).
###### 3.2 Open the docker environment:
```bash
sudo docker container start comp0244_unitree
sudo docker exec -it comp0244_unitree /bin/bash
source /opt/ros/humble/setup.bash
export ROBOT_IP="172.20.10.3"
export CONN_TYPE="webrtc"
ping 172.20.10.3
```
###### 3.3 Publish the velocity on the topic /cmd_vel to have the robot walk.
NOTE: not exceed 0.4m/s for linear velocity.
```bash
ros2 topic pub /cmd_vel -r 10 geometry_msgs/msg/Twist '{
  linear: {x: 0.2, y: 0.0, z: 0.0},
  angular: {x: 0.0, y: 0.0, z: 0.0}
}'
```
###### 3.4 IMPORTANT NOTE:
```bash
Do not forget to DISCONNECT the wifi once you finish the task
```

#### Exercises
1. Write a script that makes the robot move in a circle in the lab.
2. Write a script that makes the robot move two time in a circle in the lab and then stop.

### Task 2: Real Robot ROS bag Record
**Document:** https://support.unitree.com/home/en/developer/ROS2_service

#### Operating the Robot Using Your Own External Computer
###### 1. Following the tutorial to setup your computer network and install necessary package
[Tutorial Link](https://support.unitree.com/home/zh/developer/ROS2_service)
Remember to use **humble** in commands, not **foxy**. Here are the summary of these commands (open the docker environment):
```bash
cd /usr/app/comp0244_ws/
git clone https://github.com/unitreerobotics/unitree_ros2
apt install net-tools -y
apt install gedit
apt update && apt install ros-humble-rmw-cyclonedds-cpp && apt install ros-humble-rosidl-generator-dds-idl -y
cd unitree_ros2/cyclonedds_ws/src/
git clone https://github.com/ros2/rmw_cyclonedds -b humble && git clone https://github.com/eclipse-cyclonedds/cyclonedds -b releases/0.10.x
cd ../
colcon build --packages-select cyclonedds
source /opt/ros/humble/setup.bash
colcon build
```
Use the cable to connect the robot and your computer, and then configure the network follow the tutorial [here](https://support.unitree.com/home/zh/developer/ROS2_service)

Check connection
```bash
ping 192.168.123.99
```
And modify the ```/usr/app/comp0244_ws/unitree_ros2/setup.sh``` with the below command.

**Note**: ```eno2``` is the network card of your computer and should change:
```bash
#!/bin/bash
echo "Setup unitree ros2 environment"
source /opt/ros/humble/setup.bash
source /usr/app/comp0244_ws/unitree_ros2/cyclonedds_ws/install/setup.bash
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
export CYCLONEDDS_URI='<CycloneDDS><Domain><General><Interfaces>
                            <NetworkInterface name="eno2" priority="default" multicast="default" />
                        </Interfaces></General></Domain></CycloneDDS>'
```
Setup unitree ros2 environment
```bash
source /usr/app/comp0244_ws/unitree_ros2/setup.sh
```

###### 2. Open a new terminal and check ROS message after installing the below
```bash
source /usr/app/comp0244_ws/unitree_ros2/setup.sh
ros2 topic echo /sportmodestate
```
###### 3. Record ROS bag
```bash
mkdir /usr/app/comp0244_ws/data_comp0244
cd /usr/app/comp0244_ws/data_comp0244
ros2 bag record /lowstate /sportmodestate
```
Please move the robot. If enough data are record, please enter ```Enter ctrl+C``` to exit. The rosbag will be stored in the folder.

###### 4. Extracting ROSbag (*.db3) using python
Please change the ```bag_file``` in the script: ```extract_data_real_robot.py```
```bash
cd /usr/app/comp0244_ws/
source /opt/ros/humble/setup.sh
source unitree_ros2/setup.sh
python3 extract_data_real_robot.py
```

</details>

<details>
<summary><h2>Simulation Station (Parallel)</h2></summary>

If your group does not currently have access to the real robot, please work on the following tasks in the simulation. First, pull the latest code and compile.

### Pull recursively all repos and enter the docker to re-compile
Open the first terminal to pull recursively all repos, re-compile, and load the gazebo environment:
```bash
cd /home/$USER/comp0244_ws/comp0244-go2/
git pull --recurse-submodules
cd /home/$USER/comp0244_ws/comp0244-go2/src/FAST_LIO
git checkout -f && git checkout comp0244 && git pull
cd /home/$USER/comp0244_ws/comp0244-go2/src/waypoint_follower
git checkout -f && git checkout master && git pull
cd /home/$USER/comp0244_ws/comp0244-go2/src/local_map_creator
git checkout -f && git checkout master && git pull
cd /home/$USER/comp0244_ws/comp0244-go2/src/edge_follower
git checkout -f && git checkout master && git pull
cd /home/$USER/comp0244_ws/comp0244-go2
```

```bash
xhost +
sudo docker container start comp0244_unitree
sudo docker exec -it comp0244_unitree /bin/bash
```

```bash
source /opt/ros/humble/setup.bash

# 1. Fix the expired ROS 2 GPG key to avoid 404 Not Found errors during apt install
sudo rm -f /etc/apt/sources.list.d/ros2*.list
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
sudo apt-get update

# 2. Install RViz tools AND missing DDS dependencies required for unitree_go
sudo apt-get install ros-humble-rviz2 ros-humble-turtle-tf2-py ros-humble-tf2-ros ros-humble-tf2-tools ros-humble-rosidl-generator-dds-idl ros-humble-rmw-cyclonedds-cpp -y

# 3. Build the workspace
cd /usr/app/comp0244_ws
cd comp0244-go2/src/livox_ros_driver2 && ./build.sh humble
cd /usr/app/comp0244_ws/comp0244-go2
colcon build
source install/setup.bash
```

### Task A: Waypoints & Wall Localization
**Goal:** We will first understand how to move the robot to a waypoint (x,y,θ). We will check how to use the lidar data to fit lines to the point cloud of a wall.

#### Waypoint Follower
###### Terminal 1: Launch Gazebo and RViz
```bash
xhost +
sudo docker exec -it comp0244_unitree /bin/bash
source /usr/app/comp0244_ws/comp0244-go2/install/setup.bash
ros2 launch go2_config gazebo_mid360.launch.py
```
###### Terminal 2: Launch FAST-LIO SLAM
```bash
sudo docker exec -it comp0244_unitree /bin/bash
source /usr/app/comp0244_ws/comp0244-go2/install/setup.bash
ros2 launch fast_lio mapping.launch.py config_file:=unitree_go2_mid360.yaml
```

###### Terminal 3: Launch Waypoint Follower
```bash
sudo docker exec -it comp0244_unitree /bin/bash
source /usr/app/comp0244_ws/comp0244-go2/install/setup.bash
ros2 run waypoint_follower waypoint_follower
```

###### Terminal 4: Publish a waypoint {x, y, theta} (w.r.t the odom frame)
```bash
sudo docker exec -it comp0244_unitree /bin/bash
source /usr/app/comp0244_ws/comp0244-go2/install/setup.bash
ros2 topic pub /waypoint geometry_msgs/Pose2D "{x: 4.0, y: -2.0, theta: 0.0}" -r 1
```

**Task Exercises**
1. Move the robot to (x,y) = (0.0,1.2)
2. Publish waypoints that move the robot around the obstacle you are facing, and end up in the same pose.
3. Make the final orientation angle goal work in the code, and make it go to (0.0, 1.2, 1.6)

#### Line Detection
Keep Gazebo, SLAM, and Waypoint Follower running.
Publish a target location:
```bash
ros2 topic pub /waypoint geometry_msgs/Pose2D "{x: 0.0, y: 1.2, theta: 1.6}" -r 1
```

###### Launch Local Map Creator
```bash
sudo docker exec -it comp0244_unitree /bin/bash
cd /usr/app/comp0244_ws/comp0244-go2
source install/setup.bash
ros2 run local_map_creator local_map_creator
```

To visualize the local map, open the rviz2 window opened by Gazebo and do the following:
- Switch the Global Options to `Fixed Frame: livox`
- Add the `Marker` topic `/local_map_points`
- Add the `Marker` topic `/local_map_lines`

**Task Exercises**
1. Understand what each parameter is doing and tune them accordingly to get lines align with the point clouds when straight walls and corners appear.

### Task B: Wall Following
**Goal:** Move the Robot via waypoints and wall localization to follow the walls of a convex obstacle.

###### Terminal 1: Launch Gazebo, SLAM, Waypoint Follower
```bash
xhost +
sudo docker exec -it comp0244_unitree /bin/bash
source /usr/app/comp0244_ws/comp0244-go2/install/setup.bash
cd /usr/app/comp0244_ws/comp0244-go2/scripts
ros2 launch robot_launch.launch.py
```

###### Terminal 2: Publish a waypoint {x, y, theta} (w.r.t the odom frame)
```bash
sudo docker exec -it comp0244_unitree /bin/bash
source /usr/app/comp0244_ws/comp0244-go2/install/setup.bash
ros2 topic pub /waypoint geometry_msgs/Pose2D "{x: -1.0, y: 0.0, theta: 0.0}" -r 1
(Ctrl+C after reaching the waypoint)
```

###### Terminal 3: Follow the wall
```bash
sudo docker exec -it comp0244_unitree /bin/bash
source /usr/app/comp0244_ws/comp0244-go2/install/setup.bash
ros2 run edge_follower edge_follower
```

**Task Exercises (assuming convex obstacles)**
1. Make the robot work more smoothly based on the lines to follow the cylinder.
2. Move the robot to {x: 0.0, y: 1.0, theta: 3.14} and make it see and turn into the corners and keep following the wall cw.

</details>

---

# Lab 2: Autonomous Deployment Challenge

**Aim:** Test the autonomous path-following code in Simulation, and compete among student groups to see who can trace complex shapes on the Real Robot most efficiently and safely!

<details>
<summary><h2>I. Simulation Preview</h2></summary>

First, familiarize yourself with how the `path_follower` works in a simulation environment. The simulation acts as our sandbox.

###### 1. Start the simulation
```bash
cd /usr/app/comp0244_ws/comp0244-go2
source install/setup.bash
ros2 launch go2_config gazebo_mid360.launch.py rviz:=false
```

###### 2. Run the path_follower
```bash
cd /usr/app/comp0244_ws/comp0244-go2
source install/setup.bash
ros2 launch waypoint_follower path_following.launch.py
```
Publish different level of trajectory
```bash
ros2 run waypoint_follower publish_ellipse_shape   # easy
ros2 run waypoint_follower publish_eight_shape     # middle
ros2 run waypoint_follower publish_cosince_shape   # hard
```
Once the robot finishes its path, you can see output in the terminal:
```bash
[path_follower-3] [INFO] [1742573862.979052993] [path_follower]: ----- All waypoints reached. Stopping. ---- 
[path_follower-3] [INFO] [1742573862.979052993] [path_follower]: Sum of error: 13.896, Cost time: 27.573s
```

<img src="media/path_follower.png" alt="path_follower" width="50%">

</details>

<details>
<summary><h2>II. Real Robot Deployment Challenge</h2></summary>

This section will test out the waypoint following with the **Real-World Unitree Go2** Robot.
Teams will try to deploy predefined trajectories like an 'eight-shape', and can further extend it.

###### Task 1: Basic Deployment
Run the path_follower.py and test whether the robot can move along an eight shape.
Terminal 1:
```bash
source /usr/app/comp0244_ws/unitree_ros2/setup.sh
ros2 run waypoint_follower path_follower /utlidar/robot_odom
```
Terminal 2:
```bash
source /usr/app/comp0244_ws/unitree_ros2/setup.sh
ros2 run waypoint_follower publish_ellipse_shape 
```
Terminal 3:
```bash
source /usr/app/comp0244_ws/unitree_ros2/setup.sh
ros2 run unitree_go2_example forward_cmd_sport_mode_ctrl
```

**NOTE**: If you want to try your own state estimation algorithm, please change ```/utlidar/robot_odom``` with your own odometry topic.

**NOTE**: If you want to try your own path following algorithm, please change ```path_follower.py``` with your own functions.

###### Task 2: Advanced Shape Challenge
Using what you have learned from the `publish_eight_shape` or `publish_ellipse_shape` nodes, extend the framework to publish your own designated shapes.

- Write a publisher that generates waypoints mimicking a **"Z-shape"**. Run this on the real robot.
- Write a publisher that generates waypoints mimicking a **"Number Shape"** (e.g. your group ID). Run this on the real robot and see how accurate the real trajectory is!

</details>

---

<details>
<summary><h1>BSD 3-Clause License</h1></summary>


Copyright (c) 2016-2025 Dimitrios Kanoulas
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this
  list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice,
  this list of conditions and the following disclaimer in the documentation
  and/or other materials provided with the distribution.

* Neither the name of the copyright holder nor the names of its
  contributors may be used to endorse or promote products derived from
  this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
</details>
