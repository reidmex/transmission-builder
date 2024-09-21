# transmission-builder
Build statically linked transmission binaries

How to [build](https://blog.yobibyte.com.au/posts/build-statically-linked-transmission-daemon-for-arm64/)

-------------------------------------------------------------
**Build Statically Linked transmission-daemon  **
May 28, 2024  
In my previous posts, I demonstrated how to build a statically linked transmission-daemon with Buildah. People are asking how to build it for ARM, and I realized they don’t know that with Buildah and qemu-user-static, you can build for almost any architecture on x86_64.   In this post, I will demonstrate how to build for ARM64 and make your own custom build with the scripts in my repository.  

**Prerequisites ** 
As the build process is based on Buildah, you will only need to have Buildah, Git, and qemu-user-static installed on your system. Since they are readily available in most Linux distributions, I am not going to cover how to install them on your choice of Linux distribution. The qemu-user-static is required to build ARM64 binaries on an x86_64 system.  

**Clone the Repository **  
```
git clone https://github.com/deamen/transmission-builder.git
```

**Build the transmission-daemon Binary for ARM64**  
```
cd transmission-builder/transmission-daemon
export TRANSMISSION_VERSION=4.0.5
./build_transmission-daemon.sh arm64
```
Building for ARM64 this way will take much longer than cross-compiling on AMD64. The build time is 29 minutes, while the AMD64 build only takes 3 minutes on a free GitHub standard runner. However, I think the simplicity of the build process is worth the wait, and if you are building on a modern machine, the build time will be much shorter.  

**Build the transmission-daemon Binary for AMD64**  
```
cd transmission-builder/transmission-daemon
export TRANSMISSION_VERSION=4.0.5
./build_transmission-daemon.sh amd64
```

**Build the transmission-daemon Binary for armv7**  
```
cd transmission-builder/transmission-daemon
export TRANSMISSION_VERSION=4.0.5
./build_transmission-daemon.sh arm
```

**Build the transmission-daemon Binary for i386**  
```
cd transmission-builder/transmission-daemon
export TRANSMISSION_VERSION=4.0.5
./build_transmission-daemon.sh i386
```

**Make Your Own Custom Build**  
The build happens in a container, and the container is built from the container image defined in the transmission-builder/transmission-daemon/Containerfile.transmission file.  

You can change the CMake options to suit your needs. For example, to enable building the CLI, you will need to set -DENABLE_CLI=ON. However, any changes to the CMake options will require you to figure out the library dependencies and possibly patch the relevant CMakeLists.txt files.  

**Manual Build in the Container**  
On a successful build, the container will be removed, but the image is preserved for future customization.  

You can make your custom build in a container interactively, then make your changes to the container file once everything is working. I would suggest you have Podman installed for this purpose, so you can run the container interactively with the following command:  
```
podman run --rm -it $image_id /bin/sh
```
--------------------------------------------------------------------
