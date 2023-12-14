# Intel® RealSense™ & ArUco Marker on ROS Noetic Guide (AMD64)

This repository integrates Intel RealSense cameras with ArUco marker detection under ROS Noetic.

## 🛠️ Settings (REQUIRED) 🛠️

| Please follow instructions before starting the container.

### 1. Add udev rules for realsense in host machine.

```shell
cd /etc/udev/rules.d/
sudo nano 99-realsense-libusb.rules
``` 

the content of the rules :
```shell
echo 'SUBSYSTEM=="usb", ATTR{idVendor}=="8086", MODE="0666"' | sudo tee /etc/udev/rules.d/99-realsense-libusb.rules
 ```

reload the rules and trigger system again :

```shell
sudo udevadm control --reload-rules
sudo udevadm trigger
```

### 2. Please change hostname in `docker-compose.yaml`:

```py
## [IMPORTANT] Change container hostname to that of the host machine's 
    hostname: cleaner  
```
