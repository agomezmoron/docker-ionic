# docker-ionic
Docker image to run your ionic app against a defined Android emulador and SDK

[![](https://images.microbadger.com/badges/image/pasogo/docker-ionic.svg)](http://microbadger.com/images/pasogo/docker-ionic "Get your own image badge on microbadger.com")[![](https://images.microbadger.com/badges/version/pasogo/docker-ionic.svg)](http://microbadger.com/images/pasogo/docker-ionic)

# Table of Contents
  - [Prerequisites](#prerequisites)
  - [Build instructions](#build-instructions)
    - [Optional arguments](#optional-arguments)
    - [Possible inputs](#possible-inputs)
    - [Notes](#notes)
  - [Run instructions](#run-instructions)
    - [Mandatory arguments](#mandatory-arguments)
    - [Command specific arguments](#command-specific-arguments)
    - [Optional arguments](#optional-arguments-1)
    - [Possible inputs](#possible-inputs-1)
  - [VNC connection](#vnc-connection)
  - [Troubleshooting](#troubleshooting)
    - [Errors on the build or run step](#errors-on-the-build-or-run-step)
    - [Errors on the VNC connection](#errors-on-the-vnc-connection)


## Prerequisites

This docker image is designed to run on **Linux** systems, may not work on **Windows**. You will need at least **15GB** per build.

## Build instructions

You can build your Docker image running the Dockerfile with the following command:

Linux:
```
$ docker build -t pasogo/docker-ionic . && docker rmi -f $(docker images -f "dangling=true" -q) &> /dev/null
```

Windows:
```
docker build -t pasogo/docker-ionic . & docker rmi -f $(docker images -f "dangling=true" -q) > /dev/null
```

or pulling it from Docker:

```
docker pull pasogo/docker-ionic
```

### Optional arguments

You can specify the **Java**, **Ionic** and **Cordova** version, the **Android SDK** or the **VNC password** to use with the following variables (the values used are the default ones):

```
JAVA_VERSION=8
ANDROID_SDK_VERSION=23
VNC_PASSWD=1234
IONIC_VERSION=1.7.16
CORDOVA_VERSION=6.2.0
```

Usage:

Linux:
```
$ docker build --build-arg JAVA_VERSION=8 --build-arg ANDROID_SDK_VERSION=23 --build-arg VNC_PASSWD=1234 -t amoron/docker-appium . && docker rmi -f $(docker images -f "dangling=true" -q) &> /dev/null
```

Windows:
```
docker build --build-arg JAVA_VERSION=8 --build-arg ANDROID_SDK_VERSION=23 --build-arg VNC_PASSWD=1234 -t amoron/docker-appium . & docker rmi -f $(docker images -f "dangling=true" -q) > /dev/null
```

### Possible inputs

**Java**:
- 6
- 7
- 8

**Android SDK**:
- 19
- 20
- 21
- 22
- 23
- 24

### Notes

The second part of the command, **'&& docker rmi -f $(docker images -f "dangling=true" -q) &> /dev/null'**, is an optional one that deletes past images of the builds so the PC does not end up with several duplicated images. It can be removed without affecting the build.

## Run instructions

Run the image with the following command:

```
docker run --privileged -v /YOUR/SOURCES/FOLDER:/src --rm -t -i --net=host pasogo/docker-ionic
```

### Mandatory arguments

Please note that you **WILL** have to specify your sources folder for ionic to run.

### Command specific arguments

- **--privileged**: Allow docker to use the host's virtualization technology (KVM)
- **--net=host**: Connect the container to our local network, so we can easily access it with localhost

### Optional arguments

You can choose to update the SDK when running the image. This may be useful in some cases. Just specify it like this:

```
UPDATE_SDK="y"
```

Also, you can choose to update npm and bower plugins, or delete/add the android platform on each run:

```
UPDATE_PLUGINS="y"
UPDATE_PLATFORM="y"
```

Finally, you may specify the ionic launch command:

```
LAUNCH_COMMAND="ionic emulate -c --address YOUR_LOCAL_IP"
```

The above example is the default command that will be launched. It is recommended to add the local IP to the command.

The run command will create an Android emulator before launching it. You can specify its CPU and device with the following variables (the values used are the default ones):

```
DEVICE="Nexus S"
ABI="default/x86_64"
```

Usage:
```
docker run --privileged -v /YOUR/SOURCES/FOLDER:/src -e LAUNCH_COMMAND="YOUR_IONIC_COMMAND" -e DEVICE="Nexus S" -e ABI="default/x86_64" -e UPDATE_PLUGINS="y" --rm -t -i --net=host pasogo/docker-ionic
```

### Possible inputs

**ABIs (CPUs)**:
- armeabi-v7a
- default/x86
- default/x86_64

**Devices**: 
- tv_1080p
- tv_720p
- wear_round
- wear_round_chin_320_290
- wear_square
- Galaxy Nexus
- Nexus 10
- Nexus 4
- Nexus 5
- Nexus 6
- Nexus 7 2013
- Nexus 7
- Nexus 9
- Nexus One
- Nexus S
- 2.7in QVGA
- 2.7in QVGA slider
- 3.2in HVGA slider (ADP1)
- 3.2in QVGA (ADP2)
- 3.3in WQVGA
- 3.4in WQVGA
- 3.7 FWVGA slider
- 3.7in WVGA (Nexus One)
- 4in WVGA (Nexus S)
- 4.65in 720p (Galaxy Nexus)
- 4.7in WXGA
- 5.1in WVGA
- 5.4in FWVGA
- 7in WSVGA (Tablet)
- 10.1in WXGA (Tablet)


## VNC connection

Open **Remmina**, a built-in app in Ubuntu. Configure a new connection like this:

- **Name**: Whatever you like

- **Protocol**: VNC - Virtual Network Computing

- **Server**: localhost

- **Username**: ubuntu / 'empty'

- **Password**: Your chosen VNC password. If none was given in the build, use '**1234**'.

Click on '**Connect**' -or '**Save**' if you want to store the connection for further uses- and you should be able to see the emulator, as long as the image is up and running.

## Troubleshooting

### Errors on the build or run step

You can try first stopping and removing the images:

Linux:
```
$ docker stop $(docker ps -a -q) && docker rm -f $(docker ps -a -q)
```

Windows:
```
docker stop $(docker ps -a -q) & docker rm -f $(docker ps -a -q)
```

then deleting them with **docker rmi -f image_id**. You can check the images ids by running:

```
docker images
```

### Errors on the VNC connection

You can try first stopping and removing the images:

Linux:
```
$ docker stop $(docker ps -a -q) && docker rm -f $(docker ps -a -q)
```

Windows:
```
docker stop $(docker ps -a -q) & docker rm -f $(docker ps -a -q)
```

then running the image again and reconnecting. If the connection still fails, try restarting the docker daemon:

Linux:
```
$ sudo service docker restart
```

Windows (as admin):
```
net stop com.docker.service
net start com.docker.service
```
