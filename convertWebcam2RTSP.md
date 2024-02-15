# Convert Webcam to RTSP

In this guide, we will learn how to convert a webcam feed into an RTSP (Real-Time Streaming Protocol) stream.

## Prerequisites

Before we begin, make sure you have the following:

- A webcam connected to your computer
- VLC Media Player installed on your system

## Installation

1. [Simple RTSP Server](https://github.com/bluenviron/mediamtx/releases)
    - Unzip the downloaded file and run the 'mediamtx.exe' file.
2. [ffmpeg](https://ffmpeg.org/download.html)
    - [Step by step instructions](https://www.wikihow.com/Install-FFmpeg-on-Windows)


## Steps
### 1. Get the internal IP of WSL
Run from a WSL terminal
```sh
ip route list default | awk '{print $3}'
```
Copy the IP address that is returned. This is the IP address that will be used in the address for the RTSP stream.

### 2. RTSP Server
Start the simple rtsp server (in Windows) before running any of the ffmpeg commands if you didn't already start it during the installation step.

### 3. List connected devices
```sh
ffmpeg -list_devices true -f dshow -i dummy
```
Copy the exact device name of the webcam you want to stream.

### 4. Ensure configuration
Review the window where you are running the Simple RTSP server and ensure the RTSP listener is opened and ready to receive the stream.

### 5. Start streaming
Replace the `<ip>` with the IP address you copied in step 1 and the `<video>` with the device name you copied in step 3.
```sh
ffmpeg -f dshow -i video="<video>" -framerate 30 -video_size 640x480 -f rtsp -rtsp_transport udp rtsp://<ip>:8554/webcam.h264
```

### 6. View the stream
In the Simple RTSP server window, you should now see the stream being received and processed.  Note that the stream is published on path 'webcam.h264'.

- Open VLC Media Player and click on `Media` > `Open Network Stream`.
- On the network tab, enter the streaming RTSP endpoint which should look similar to this:
```sh
rtsp://<ip>:8554/webcam.h264
```
- Click `Play` and you should now see the webcam feed being streamed in VLC Media Player.
