# 🧠 Installation Guide: Ubuntu 22.04.5 LTS, ROS 2 (Humble Hawksbill), and Ignition Fortress (Gazebo) 🛠

Welcome to the **ROS 2 Installation Guide** — a comprehensive step-by-step tutorial to set up **Ubuntu 22.04.5 LTS (Jammy Jellyfish)**, **ROS 2**, **Nav2**, **MoveIt 2**, and **Ignition Fortress (Gazebo)** for robotics development. This guide is tailored for the **eYRC Theme** and general ROS 2 projects, ensuring a stable and efficient environment.

---

## ⚙️ Minimum System Requirements

To run **ROS 2**, **Nav2**, **MoveIt 2**, and **Ignition Fortress** smoothly on a system (dual-booted with Windows or standalone Ubuntu), ensure the following:

| Component | Minimum | Recommended |
|------------|----------|-------------|
| **Processor** | 64-bit Dual-Core | Quad-Core Intel i5 / Ryzen 5 or higher |
| **RAM** | 4 GB | 8 GB + |
| **Storage** | 40 GB free | 80 GB + SSD preferred |
| **GPU** | Integrated | NVIDIA GPU (CUDA 10+ optional for simulation) |

> 💡 *Do NOT use Virtual Machine (VM) or Windows Subsystem for Linux (WSL) — they limit performance and hardware access, which are essential for ROS 2 navigation, motion planning, and Gazebo simulations.*

---

## 🐧 Step 1: Download Ubuntu Image

Get the official **Ubuntu 22.04.5 LTS (Jammy Jellyfish)** ISO image from the official site below. Save it to a memorable location on your PC.

🔗 [**Download Ubuntu 22.04.5 LTS (Desktop 64-bit)**](https://releases.ubuntu.com/jammy/ubuntu-22.04.5-desktop-amd64.iso)

---

## 💽 Step 2: Create a Bootable USB Stick

To install Ubuntu Desktop, you need to **write the downloaded ISO file** to a USB stick — not just copy it. Use one of these free tools:

- 🧰 [**Rufus**](https://rufus.ie/)  
- 💿 [**balenaEtcher**](https://www.balena.io/etcher/)

> 🔧 Use a USB drive of at least **8 GB** capacity.  
> Recommended to use **balenaEtcher**. This process will **erase** the USB contents, so back up important data first.

---

## 🖥️ Step 3: Prepare for Dual-Boot (Windows Users)

For dual-boot with Windows, prepare an **unallocated disk section** of at least **50 GB** for Ubuntu:  
1. Open **Disk Management** in Windows (right-click Start menu > Disk Management).  
2. Select a partition with sufficient free space, right-click, and choose **Shrink Volume**.  
3. Enter at least **50,000 MB** (50 GB) to create unallocated space.  
4. Leave this unallocated space for Ubuntu to use during installation.

> ⚠️ **Warning:** Be cautious when modifying partitions. Back up important data before proceeding. If you face issues, report them in the repository for assistance—do not make decisions if unsure.
> 
---

## 🖥️ Step 4: Boot from the USB Drive

1. Insert the bootable USB into your PC or laptop.  
2. Restart your system and press **F12** during startup to open the boot menu.  
   - Common keys: **F12**, **F10**, **F2**, or **Esc** (varies by manufacturer).  
3. Select your USB device from the boot options.  
4. Choose **“Install Ubuntu”** when prompted.

---

## ⌨️ Step 5: Choose Installation Settings

- Select your **keyboard layout**.  
- Choose **Normal Installation** and check:
  - ✅ *Download updates while installing Ubuntu*
  - ✅ *Install third-party software (recommended)*

---

## 💾 Step 6: Disk Partitioning

Choose one of the following:

- **Erase Disk and Install Ubuntu** → for a fresh install.  
- **Install Ubuntu Alongside Windows** → for **dual boot** setup (recomended).  
- **Something Else** → for manual/custom partitions (advanced users).

> ⚠️ **Warning:** “Erase Disk” will permanently delete all data.  
> Make sure to **back up important files** before proceeding.

---

## 🌍 Step 7: Set Location and User Details

- Set your **timezone**, **username**, and **password**.  
- These credentials will be needed to log in and run system commands.

---

## ☕ Step 8: Relax and Let Ubuntu Install

Sit back and let the installation process complete. Once finished, **remove the USB stick** when prompted and restart your computer.

You’ll now have **Ubuntu 22.04.5 LTS** ready to go!

---

### 🎥 Video Reference

If you’re setting up a **dual-boot** system, watch this detailed walkthrough:  
📺 [**Dual Boot Ubuntu with Windows — Step by Step Guide**](https://youtu.be/GXxTxBPKecQ)

> ⚠️ Some steps may vary slightly depending on your system configuration, but following this guide carefully ensures a smooth installation.

---

## 🤖 Step 9: Install ROS 2 (Humble Hawksbill)

### What is ROS 2?

**Robot Operating System (ROS)** is a set of software libraries and tools for building robot applications. From drivers and state-of-the-art algorithms to powerful developer tools, ROS has the open-source tools you need for your robotics project.

### Update Your System

After installing Ubuntu, update your system to the latest version. Open a terminal (`Ctrl+Alt+T`) and run:

```bash
sudo apt update
sudo apt upgrade
```

### Configure UTF-8 Locale

Ensure your system is set to use UTF-8 encoding:

```bash
locale  # Check for UTF-8

sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # Verify settings
```

### Setup ROS 2 Apt Repository

Add the ROS 2 apt repository to your system. First, ensure the Ubuntu Universe repository is enabled:

```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
```

Install the `ros2-apt-source` package to configure ROS 2 repositories:

```bash
sudo apt update && sudo apt install curl -y
export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F" '{print $4}')
curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo $VERSION_CODENAME)_all.deb"
sudo dpkg -i /tmp/ros2-apt-source.deb
```

### Install ROS 2 Packages

Update your apt repository caches:

```bash
sudo apt update
sudo apt upgrade
```

Install the **ROS 2 Humble Desktop** package (includes ROS, RViz, demos, and tutorials):

```bash
sudo apt install ros-humble-desktop
```

Install development tools for building ROS packages:

```bash
sudo apt install ros-dev-tools
```

### Environment Setup

To automatically source ROS 2 environment variables in every new terminal session:

```bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
```

### Try a Demo: Talker-Listener

To verify your ROS 2 installation, try the talker-listener demo. In one terminal, source the setup file and run a C++ talker:

```bash
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_cpp talker
```

In another terminal, source the setup file and run a Python listener:

```bash
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_py listener
```

You should see the talker publishing messages and the listener receiving them, verifying that both C++ and Python APIs are working correctly. 🥳

For more information, visit: https://docs.ros.org/en/humble/Installation.html

---

## 🏰 Step 9: Install Ignition Fortress (Gazebo)

**Ignition Fortress** is a modern robotics simulator (commonly referred to as Gazebo) that integrates well with ROS 2 for simulation tasks.

### Install Necessary Tools

First, install the required tools:

```bash
sudo apt-get update
sudo apt-get install lsb-release gnupg
```

### Install Ignition Fortress

Add the Gazebo repository and install Ignition Fortress:

```bash
sudo curl https://packages.osrfoundation.org/gazebo.gpg --output /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/gazebo-stable.list > /dev/null
sudo apt-get update
sudo apt-get install ignition-fortress
```

All libraries should be ready to use, and the `ign gazebo` application is now ready to be executed.

### Install ROS 2 Gazebo Integration

To ensure compatibility between ROS 2 and Ignition Fortress, install the ROS-Gazebo bridge:

```bash
sudo apt-get install ros-humble-ros-gz
```

This package provides the necessary libraries to integrate ROS 2 with Ignition Fortress for your simulations.

---

## 🚀 Next Steps

- For now complete these setups. we will move to the next part soon 

