# Containerized build & test utilities

This folder contains scripts and dockerfiles used to build and test MXNet using Docker containers

You need docker and nvidia docker if you have a GPU.

If you are in ubuntu an easy way to install Docker CE is executing the following script:


```
#!/bin/bash
set -e
set -x
export DEBIAN_FRONTEND=noninteractive
apt-get -y install curl
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
         stable"
apt-get update
apt-get -y install docker-ce
service docker restart
usermod -a -G docker $SUDO_USER
```

For detailed instructions go to the docker documentation.


## build.py

The main utility to build is build.py which will run docker and mount the mxnet folder as a volume
to do in-place builds.

The build.py script does two functions, build the docker image, and it can be also used to run
commands inside this image with the propper mounts and paraphernalia required to build mxnet inside
docker from the sources on the parent folder.

A set of helper shell functions are in `functions.sh`. `build.py --help` will display usage
information about the tool.

To build for armv7 for example:

```
./build.py -p armv7 /work/functions.sh build_armv7
```

## Warning
Due to current limitations of the CMake build system creating artifacts in the source 3rdparty
folder of the parent mxnet sources concurrent builds of different platforms is NOT SUPPORTED.
