# Raspbian Jessie with ROS and Cloud9 IDE
The below steps are to setup raspberry pi with raspbian-lite headless (no UI) and connects to your WI-FI, SSH enabled on initial boot.

## Preparing Raspberry pi headless installation
### Download raspbian lite from below link
http://downloads.raspberrypi.org/raspbian_lite/images/raspbian_lite-2017-07-05/

### Writing an image to the SD card
Use an image writing tool to write image on SD card.

Download [Etcher](https://etcher.io/) a graphical SD card writing tool that works on Mac OS, Linux and Windows, and is the easiest option for most users. Etcher also supports writing images directly from the zip file, without any unzipping required. To write your image with Etcher:

- Connect a SD card reader with the SD card inside.
- Open Etcher and select from your hard drive the Raspberry Pi .img or  .zip file you wish to write to the SD card.
- Select the SD card you wish to write your image to.
- Review your selections and click 'Flash!' to begin writing data to the SD card.

### Prepare SSH and Wi-Fi settings (don't boot Pi yet)
Eject the SD card and insert it back into PC/Mac again. 
It will show up as `Boot` external drive
- Double click to open / mouse right click open
- create a file named as `ssh` without any file extension and save it in SD Card
- create another file and named as `wpa_supplicant.conf` (make sure file extension is `.conf`)
- Copy paste the below code, put your wifi name, password and save file
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US

network={
    ssid="your wi-fi name"
    psk="wi-fi password"
    }
```
`NOTE: the file has to be place in SD card and when it's boots 1st time it will auto configure you Pi Wi-Fi and ssh enable`
> Save the changes and Eject the SD card

Insert the SD card in Raspberry Pi and Boot it ( this should work on all versions of Pi) - I tested with Pi3, Zero.

Raspberry Pi should boot up and connects to your network. To see it's connected to your network open browser, goto 192.168.1.1 your router network URL.

At this point, you should be able to login to Pi without any keyboard and monitor connected to it. `yay!!!!`

## Install ROS (Robotics Operating Systems) Framework 
### (only for Raspbian Jessie)

Here are the detailed steps in to [install ROS](http://wiki.ros.org/ROSberryPi/Installing%20ROS%20Kinetic%20on%20the%20Raspberry%20Pi). However, let us do a quick run through again.
### Steps to install
```
$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
$ sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install -y python-rosdep python-rosinstall-generator python-wstool python-rosinstall build-essential cmake
$ sudo rosdep init
$ rosdep update
$ mkdir -p ~/ros_catkin_ws
$ cd ~/ros_catkin_ws
$ rosinstall_generator ros_comm --rosdistro kinetic --deps --wet-only --tar > kinetic-ros_comm-wet.rosinstall
$ wstool init src kinetic-ros_comm-wet.rosinstall
$ mkdir -p ~/ros_catkin_ws/external_src
$ cd ~/ros_catkin_ws/external_src
$ wget http://sourceforge.net/projects/assimp/files/assimp-3.1/assimp-3.1.1_no_test_models.zip/download -O assimp-3.1.1_no_test_models.zip
$ unzip assimp-3.1.1_no_test_models.zip
$ cd assimp-3.1.1
$ cmake .
$ make
$ sudo make install
$ cd ~/ros_catkin_ws
```
```$ rosdep install -y --from-paths src --ignore-src --rosdistro kinetic -r --os=debian:jessie ```

If this fails or throws error message try below. 

```$ rosdep install --from-paths src --ignore-src --rosdistro kinetic -y``` 

Countiue....

```
$ sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/kinetic -j2
$ source /opt/ros/kinetic/setup.bash
$ echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc 
```

> ROS Installation complete.

## C9SDK installation 
We all know how hard and easy to use IDEs are. I really like nano editor that built it and comes with Linux. However, won't it be great to use a really good IDE like c9 where you can open all files in one view, update them parallel, auto indentation, etc... which boosts productivity and coding efficiency.

To get started with we need to install
- npm
- nodejs
- C9Core/SDK (cloud9 git repo)

Installing [nodejs](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions)
```
$ curl --silent --location https://rpm.nodesource.com/setup_9.x | sudo bash -
$ sudo yum -y install nodejs
```
git clone/download c9 repo, build and install [steps](https://github.com/c9/core)
```
$ git clone https://github.com/c9/core.git c9sdk
$ cd c9sdk
$ scripts/install-sdk.sh
```

> c9 Installation complete

Launch IDE and access it from an system connected to your wi-fi by trying this command.
```
$ node server.js -p 5100 -l 0.0.0.0 -a pi:rasoberry -w ~
```
Now visit `http://raspberryPi:5100/ide.html` in web browser to load Cloud9.
