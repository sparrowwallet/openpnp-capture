![OpenPNP Logo](https://raw.githubusercontent.com/openpnp/openpnp-logo/develop/logo_small.png)

# openpnp-capture

OpenPnP Capture is a cross platform video capture library with a focus on machine vision. It's goals are:

* Native camera access on Windows, Linux and Mac. Implemented with DirectShow, V4L2 and AVFoundation respectively.
* Named device enumeration.
* Strong, repeatable unique IDs.
* Format control with support for at least YUV and MJPEG. MJPEG will allow the use of multiple USB cameras on a single hub.
* Auto and manual exposure control.
* Auto and manual focus control.
* Simple, common C wrapper for the listed APIs.

# Status

### Windows

| Feature       | Implemented   |
| ------------- |:-------------:|
| Device Enumeration | Yes |
| Capturing | Yes |
| MJPEG formats | Yes |
| YUV formats | Yes |
| Exposure control | Yes |
| Focus control | Yes / Untested |
| Zoom control | Yes |
| Gain control | Yes |
| White balance control | Yes |
| Framerate control | No |

### Linux

| Feature       | Implemented   |
| ------------- |:-------------:|
| Device Enumeration | Yes |
| Capturing | Yes |
| MJPEG formats | Yes |
| YUV formats | Yes, YUYV/YUV2 |
| Exposure control | Yes |
| Focus control | Yes / Untested |
| Zoom control | Yes |
| Gain control | Yes / Untested |
| White balance control | Yes |
| Framerate control | No |

### OSX

| Feature       | Implemented   |
| ------------- |:-------------:|
| Device Enumeration | Yes |
| Capturing | Yes |
| MJPEG formats | Yes (dmb1) |
| YUV formats | Yes |
| Exposure control | Yes / Experimental |
| Focus control | Yes / Experimental |
| Zoom control | Yes / Experimental |
| Gain control | Yes / Experimental |
| White balance control | Yes / Experimental |
| Framerate control | No |

# TODO
* support for re-enumeration.

# Building OpenPnP Capture

### Dependencies
* CMAKE 3.1 or better
* MAKE (osx, linux)
* Visual Studio 2013 + NMake or Ninja Build (windows)
* NASM for building libjpeg-turbo (linux)

### Build instructions (Windows)
Run the batch file 'bootstrap.bat' and choose the desired build system (VisualStudio/nmake or Ninja). Make sure the compiler (Visual Studio) is in the search path. 

Go to the build directory and run nmake or ninja to build the library and the test application.

### Build instructions (OSX)
Run 'bootstrap_osx.sh'. Run make.

### Build instructions (Linux) (works for ARM64 targets such as RPI3 too)
Run 'bootstrap_linux.sh'. Run make.

### Docker

For instance, to cross-compile for a RaspberryPi 3:

```
$ git clone https://github.com/openpnp/openpnp-capture && cd openpnp-capture
$ docker build --tag openpnp-capture-rpi3-arm64:latest . -f docker/Dockerfile.rpi3_arm64
$ docker run -it -v $PWD:/openpnp-capture openpnp-capture-rpi3-arm64 ./bootstrap_linux.sh
```

## Supporting other platforms
* Implement all PlatformXXX classes, like in the win or linux directories.
* PlatformContext handles device and internal frame buffer format enumeration.
* PlatformStream is responsible for capturing and decoding the camera stream to a 8-bit per channel RGB frame buffer.
* Statically link external dependencies.
* Tweak .travisci.yml accordingly for it to be released automatically.

# Releases

Releases are built automatically for all supported platforms. See https://github.com/openpnp/openpnp-capture/releases/latest to download the latest.

# Platform Notes

## MacOS

On MacOS as of 10.15 Camera permission is needed to open the camera. The library will automatically
execute the permission request, but an Info.plist is required to exist in the application bundle.
An example Info.plist is:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>NSCameraUsageDescription</key>
	<string>openpnp-capture needs permission to access the camera to capture images.</string>
</dict>
</plist>
```

You can reset the camera permissions in MacOS for testing purposes by running ` tccutil reset Camera`.
