---
title: "A libcamera port of Plasma Camera"
date: 2025-07-07
draft: false
tags: [kde]
---

Cameras!

<img src="/images/blog/2025/07/plasma-camera/app.png" width="200px" />

Today, I am announcing a new release of [Plasma Camera](https://invent.kde.org/plasma-mobile/plasma-camera), a camera application for Plasma Mobile (though it can also be used on desktop!). This release ports the application to use [libcamera](https://libcamera.org/) as the backend for interfacing with cameras, finally allowing for it to be used on Linux mobile devices (such as the OnePlus 6).

The main porting work was done by my friend [Andrew (koitu)](https://koitu.com) a couple of months ago. It remained stalled on some issues, so I picked it up in the past week to complete the port and finish the application. He will have a blog post that goes into the more technical details of the port (linked here once it's ready).

# Background

Cameras have been a long neglected area in Plasma Mobile, ever since the focus shifted from [halium](https://halium.org/) to mainline devices. With mainline devices, libcamera drivers have been developed for them, allowing for cameras to be used in applications over Pipewire (ex. [GNOME Snapshot](https://apps.gnome.org/Snapshot/), Firefox, Chromium).

[Plasma Camera](https://invent.kde.org/plasma-mobile/plasma-camera) was originally created in 2019 with halium devices in mind, using the official [Qt Camera](https://doc.qt.io/qt-6/cameraoverview.html) library as a backend for interfacing with cameras. This library allows for the app to work on Android and on desktop with USB webcams. Unfortunately, Qt Camera does not currently have support for using Pipewire or libcamera directly as a backend, and so is unable to interface with the cameras on the OnePlus 6 and Pixel 3a.

# Porting Plasma Camera

Qt Camera is a fairly high-level API designed to abstract over many different platforms, beyond Linux. Since our focus is on Linux, we decided to take this chance to port [Plasma Camera](https://invent.kde.org/plasma-mobile/plasma-camera) to use libcamera directly for best control over the camera pipeline and features. Note that this approach differs from some other camera applications that use **Pipewire**, which has a backend to communicate with libcamera.

The [API](https://libcamera.org/api-html/index.html) for libcamera is fairly comprehensive.

In order to implement the viewfinder (camera preview), we create a worker thread that is responsible for polling the camera for frames. A series of "requests" with a framebuffers allocated to each were created, which we cycle through when polling for frames. Libcamera then gives us a frame for each poll request, in which we send to our application thread to display.

For simplicity, [Qt Multimedia](https://doc.qt.io/qt-6/qtmultimedia-index.html) was used for media processing. Frames from libcamera are wrapped in [QImage](https://doc.qt.io/qt-6/qimage.html)s and sent to a [QVideoSink](https://doc.qt.io/qt-6/qvideosink.html) to be displayed in the UI. Any transformations needed (such as rotation correction due to how sensors are mounted on phones, or mirroring for front-facing cameras) are done before the frame is added to the sink. For taking photos and videos, we reuse the viewfinder's frames.

For photos, we simply write the [QImage](https://doc.qt.io/qt-6/qimage.html) to the disk.

Videos are much more tricky. Using [Qt Multimedia](https://doc.qt.io/qt-6/qtmultimedia-index.html) we can build a video processing pipeline. We create a [QMediaCaptureSession](https://doc.qt.io/qt-6/qmediacapturesession.html) to facilitate all of the inputs and outputs needed. We then attach a media recorder [QMediaRecorder](https://doc.qt.io/qt-6/qmediarecorder.html) for writing the video, an audio input ([QAudioInput](https://doc.qt.io/qt-6/qaudioinput.html)) and a video input ([QVideoFrameInput](https://doc.qt.io/qt-6/qvideoframeinput.html)). We have a separate polling timer that polls at the framerate of the video (which can differ from the framerate of the viewfinder), copying frames one-by-one into the QVideoFrameInput instance (more on this later) to be encoded by QMediaRecorder.

In the future, it may make sense to investigate whether we could benefit from porting to using [GStreamer](https://gstreamer.freedesktop.org/) directly for media processing. We currently use Qt Multimedia with its ffmpeg backend. While Qt Multimedia does have an gstreamer backend, it has some [limitations](https://doc.qt.io/qt-6/qtmultimedia-gstreamer.html) and was thus removed from being the default backend as a result.

## UI work

I also took the liberty of doing some substantial refactoring and reworking of the UI code. We dropped some camera settings for the initial port of the application, to be restored later. However some other features were introduced.

The application has these features:
- Photo capture
- Video capture
- Audio recording toggle for video capture
- EV setting (exposure value)
- Captured photo/video preview 
- Video recording settings (codec, resolution, FPS, quality)
- Timer before taking a photo
- Warnings for when the encoder is detected to not be keeping up with the video stream
- Settings persistence

<img src="/images/blog/2025/07/plasma-camera/settings.png" width="200px" />

# Results

With USB webcams, both photo capture and video recording work.

<img src="/images/blog/2025/07/plasma-camera/webcamshot.jpg" width="200px" />

<img src="/images/blog/2025/07/plasma-camera/webcamscreenshot.png" width="200px" />

It also *sort of* works on phones. I tested on the OnePlus 6 and Pixel 3a. I suspect that most of the issues are simply due to the camera driver not yet being mature enough, as I can replicate most of the issues on other camera applications. The photo quality and colours are not optimal, and there appears to be a fixed focal length, and so far away things look blurry.

The viewfinder stream is fine on my OnePlus 6 and looks smooth. However, for my Pixel 3a, the frames start flashing light and dark colours when I point the camera at any bright light source. I suspect it is due to the camera driver overcompensating for exposure perhaps? Not sure 😅

Photo capture works on both devices, outputting the frame from the viewfinder at full resolution to the disk almost instantly. Though the quality of the pictures is reminiscent of early 2000s phone photography.

<img src="/images/blog/2025/07/plasma-camera/pixel3a-1.jpg" width="200px" />
<img src="/images/blog/2025/07/plasma-camera/op6-1.jpg" width="200px" />

The video recording experience however isn't quite usable unfortunately, the video encoder does not appear to be able to keep up.

<video width="216" height="284" controls>
    <source src="/images/blog/2025/07/plasma-camera/video.mp4" type="video/mp4">
</video>

# Limitations

## Video recording issues on phones

The main barrier to video recording seems to be the performance of the video encoder. I've noticed on both phones that many frame calls to [QVideoFrameInput](https://doc.qt.io/qt-6/qvideoframeinput.html) fail because [QMediaRecorder](https://doc.qt.io/qt-6/qmediarecorder.html)'s queue is simply full and cannot keep up with the amount of frames coming in. This can be mitigated somewhat by playing with the video recording settings. I've generally found the MPEG2 codec to be substantially faster for devices, though it gives very ugly artifacting at low quality, and sometimes gives an error. Of course, lowering the resolution and FPS also can help too.

For each frame given to [QVideoFrameInput](https://doc.qt.io/qt-6/qvideoframeinput.html), I also set its [timestamp](https://en.wikipedia.org/wiki/Presentation_timestamp) to ensure that the encoder places it at the correct place. However, when we start dropping frames due to the encoder being full, we end up with gaps in the video without a frame, which I suspect is what is causing the pixelated "corrupt video" effect (though it only happens with H264, and not MPEG2 encoding?). We cannot really queue frames for the encoder, because we would very quickly run out of memory. I have an [open issue](https://invent.kde.org/plasma-mobile/plasma-camera/-/issues/16) about this since I am not really sure how to address it yet.

<img src="/images/blog/2025/07/plasma-camera/videoslow.png" width="200px" />

## Rotation issues

Device rotation can be a bit of a problem with the application right now. We already account for the screen orientation in comparison to the camera orientation, which is reported as a [property](https://libcamera.org/api-html/namespacelibcamera_1_1properties.html#a3b25103242b577663c87eb8d492fe94e) by libcamera.

The viewfinder however can be a problem when the display rotation is different from the screen's orientation (ex. rotated 90, 180, 270 degrees). This is done by the compositor (ex. KWin), the application only sees that the window size has changed. However, that means the viewfinder is rotated as well! We are able to adjust for this in taken photos and video by reading the rotation sensor data (with [QOrientationSensor](https://doc.qt.io/qt-6/qorientationsensor.html)/iio-sensor-proxy), however we cannot do the same for the viewfinder because we don't know which orientation the compositor has the application in, which could be different from the sensor due to rotation-lock and manual settings.

I recommend keeping an **orientation lock** on "portrait" mode when using the application on a phone until we find a fix, that way the viewfinder does not get mismatched from what you see. We are tracking this issue here: https://invent.kde.org/plasma-mobile/plasma-camera/-/issues/14

<img src="/images/blog/2025/07/plasma-camera/rotationbad.png" width="400px" />

## Missing camera controls

The drivers for the OnePlus 6 and Pixel 3a seem to be missing almost all of the libcamera [controls](https://libcamera.org/api-html/namespacelibcamera_1_1controls.html). At least, calling `camera->controls()` ([doc](https://libcamera.org/api-html/classlibcamera_1_1Camera.html#a10ef308df9bfb61c9ad825f4580074a7)) gives only the [Contrast](https://libcamera.org/api-html/namespacelibcamera_1_1controls.html#a76f39d8c7048f1cea3af63bd2839d6b4) control from libcamera. There are other controls that I would like to implement once they become available, such as [focus windows](https://libcamera.org/api-html/namespacelibcamera_1_1controls.html#aeece9e5e8fd738c5446efbdb06f93aac).

Once these are implemented in the driver (or if it's fixed as an issue on our side) and support is added in the application, we will have a lot more camera features to play with!

# Conclusion

We finally have a base on to use for the camera stack on Linux mobile. I hope the application continues to improve as drivers and camera support get better over time on these devices.

So, give it a try! And feel free to come join us to talk about it in the [Plasma Mobile matrix channel](https://matrix.to/#/%23plasmamobile:matrix.org)!
