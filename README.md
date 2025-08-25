# 🚗 CARLA Unreal Engine 4 Installation Guide

Guide for installing CARLA on **Ubuntu 22.04** using **Unreal Engine 4**

---

## 📋 Table of Contents

* [💻 Recommended System](#-recommended-system)
* [⚡ Quick Installation](#-quick-installation)  *(Recommended for quick CARLA setup)*
* [🛠️ Installation (Building from Source)](#-installation-building-from-source)
* [🔗 References](#-references)

---

## 💻 Recommended System

* Intel i7/i9 9th–11th gen, or AMD Ryzen 7/9
* 32 GB RAM or more
* NVIDIA RTX 3070/3080/3090/4090 (16 GB VRAM or more)
* Ubuntu 22.04 (Windows 11 is supported, but this guide covers Linux)

---

## ⚡ Quick Installation

Quick setup using binary files without building from source.

### **Step 1: Download CARLA Binary**

Download from here: [CARLA\_0.9.15.tar.gz](https://tiny.carla.org/carla-0-9-15-linux)

### **Step 2: Setup Directory and Extract Archive**

```bash
cd ~/Downloads
cp CARLA_0.9.15.tar.gz ~/
mkdir -p ~/carla_simulator
tar -xvzf CARLA_0.9.15.tar.gz -C ~/carla_simulator
```

### **Step 3: Install CARLA Python API**

Download the CARLA Python API:

* [Wheel (.whl)](https://github.com/gezp/carla_ros/releases/download/carla-0.9.15-ubuntu-22.04/carla-0.9.15-cp310-cp310-linux_x86_64.whl)
* [Egg (.egg)](https://github.com/gezp/carla_ros/releases/download/carla-0.9.15-ubuntu-22.04/carla-0.9.15-py3.10-linux-x86_64.egg)

Copy files:

```bash
cp ~/Downloads/carla-0.9.15-cp310-cp310-linux_x86_64.whl ~/carla_simulator/PythonAPI/carla/dist/
cp ~/Downloads/carla-0.9.15-py3.10-linux-x86_64.egg ~/carla_simulator/PythonAPI/carla/dist/
```

Install API:

```bash
cd ~/carla_simulator/PythonAPI/carla/dist/
pip install carla-0.9.15-cp310-cp310-linux_x86_64.whl
```

Test installation:

```bash
python3 -c "import carla; print('CARLA Python API installed successfully!')"
```

### **Step 4: Update Environment Variables**

Edit your `~/.bashrc`:

```bash
code ~/.bashrc
```

Add paths:

```bash
export CARLA_ROOT=$HOME/carla_simulator
export PYTHONPATH=$PYTHONPATH:${CARLA_ROOT}/PythonAPI/carla/dist/carla-0.9.15-py3.10-linux-x86_64.egg
```

Apply changes:

```bash
source ~/.bashrc
```

### **Step 5: Run CARLA**

```bash
cd ~/carla_simulator
./CarlaUE4.sh -prefernvidia
```

![CARLA Server](./images/carlar_ue4_package.png)

---

## 🛠️ Installation (Building from Source)

### **Step 1: Install Software Requirements**

```bash
sudo apt-add-repository "deb http://archive.ubuntu.com/ubuntu jammy main universe"
sudo apt-get update
sudo apt-get install build-essential clang-10 lld-10 g++-12 cmake ninja-build libvulkan1 python python3 python3-dev python3-pip libpng-dev libtiff5-dev libjpeg-dev tzdata sed curl unzip autoconf libtool rsync libxml2-dev git git-lfs
sudo update-alternatives --install /usr/bin/clang++ clang++ /usr/lib/llvm-10/bin/clang++ 180 &&
sudo update-alternatives --install /usr/bin/clang clang /usr/lib/llvm-10/bin/clang 180 &&
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-12 180
```

### **Step 2: Install Python Dependencies**

```bash
pip install --user setuptools wheel distro auditwheel==4.0.0
pip3 install --user setuptools wheel distro
```

### **Step 3: Clone Unreal Engine 4**

[Link GitHub account to Epic Games](https://www.unrealengine.com/en-US/ue-on-github)

```bash
git clone --depth 1 -b carla https://{YOUR_TOKEN}@github.com/CarlaUnreal/UnrealEngine.git ~/UnrealEngine_4.26
```

Replace `{YOUR_TOKEN}` with your GitHub token.

### **Step 4: Build and Launch Unreal Engine**

```bash
cd ~/UnrealEngine_4.26
./Setup.sh && ./GenerateProjectFiles.sh && make
```

Test Unreal Editor:

```bash
cd ~/UnrealEngine_4.26/Engine/Binaries/Linux
./UE4Editor
```

![Unreal Engine 4 Editor](./images/unreal4.png)

### **Step 5: Clone CARLA**

```bash
git clone -b ue4-dev https://github.com/carla-simulator/carla
```

### **Step 6: Download Assets**

```bash
cd carla
./Update.sh
```

### **Step 7: Set Unreal Engine Variable**

Edit `.bashrc`:

```bash
export UE4_ROOT=~/UnrealEngine_4.26
```

Apply changes:

```bash
source ~/.bashrc
```

### **Step 8: Build CARLA**

Build Python API:

```bash
cd carla
make PythonAPI
```

Set Python paths:

```bash
export CARLA_ROOT=$(pwd)
export PYTHONPATH=$PYTHONPATH:$CARLA_ROOT/PythonAPI/carla/dist/carla-0.9.15-py3.10-linux-x86_64.egg:$CARLA_ROOT/PythonAPI/carla
```

Install Python package:

```bash
cd PythonAPI/carla/dist
pip install carla-0.9.15-cp310-cp310-linux_x86_64.whl
```

Launch CARLA Editor:

```bash
cd carla
make launch
```

![CARLA Unreal Editor](./images/carlar_unreal4.png)

Create standalone build:

```bash
make package
```

Run standalone CARLA:

```bash
cd Dist/CARLA_Shipping*/LinuxNoEditor
./CarlaUE4.sh -prefernvidia
```

![CARLA Server](./images/carlar_ue4_package.png)

### **Step 9: ROS2 Integration**

Refer to [CARLA ROS2 Bridge documentation](https://carla.readthedocs.io/en/latest/ros2_bridge/).

Example repo: [Carlar\_ros2](https://github.com/peeradonmoke2002/Carlar_ros2.git)

---

## 🔗 References

* [📚 CARLA Documentation](https://carla.readthedocs.io/en/latest/)
* [💻 CARLA GitHub](https://github.com/carla-simulator/carla)
